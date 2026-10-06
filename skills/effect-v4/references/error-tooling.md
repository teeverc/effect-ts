# Error Tooling (Cause-level handling, sandbox, ErrorReporter)

Use when you need to see or react to **everything** that can go wrong — typed failures, defects and
interruptions — e.g. at HTTP/RPC/worker boundaries, for logging, or for error tracking.
Data model of `Cause` / `Exit`: `exit-cause.md`. Typed-error handling: `error-management.md`.

## Cause-level recovery

```ts
import { Cause, Effect } from "effect"

const risky = Effect.sync(() => {
  throw new Error("boom")          // thrown in sync → defect
})

// every failure: typed errors, defects, interrupts (v3: catchAllCause)
const a = risky.pipe(
  Effect.catchCause((cause) => Effect.succeed(Cause.pretty(cause)))
)

// defects only (v3: catchAllDefect)
const b = risky.pipe(
  Effect.catchDefect((defect) => Effect.succeed(`defect: ${String(defect)}`))
)

// predicate over the whole Cause
const c = risky.pipe(
  Effect.catchCauseIf(Cause.hasDies, (cause) => Effect.succeed(Cause.squash(cause)))
)

// Filter over the Cause; Cause.find* functions return Result and work as Filters.
// Handler gets (selectedValue, originalCause); non-matching causes re-fail with the residual.
const d = risky.pipe(
  Effect.catchCauseFilter(Cause.findDefect, (defect, _cause) => Effect.succeed(String(defect)))
)
```

- `Effect.catchCause(f)` — handles all causes (does not distinguish; inspect `cause.reasons`).
- `Effect.catchDefect(f)` — only `Die` reasons; typed errors and interrupts pass through.
- `Effect.catchCauseIf(predicate, f)` / `Effect.catchCauseFilter(filter, f)` (v3 `catchSomeCause`).
- Useful filters: `Cause.findError` (first typed error), `Cause.findFail`, `Cause.findDefect`, `Cause.findDie`,
  `Cause.findInterrupt`, `Cause.filterInterruptors` — all `(cause) => Result<Found, Cause>`.
- To transform a cause (v3 `mapErrorCause`): `Effect.catchCause((cause) => Effect.failCause(Cause.map(cause, f)))`.

## Observing without recovering

- `Effect.tapCause(f)`, `Effect.tapCauseIf(pred, f)`, `Effect.tapCauseFilter(filter, f)`, `Effect.tapDefect(f)`,
  `Effect.tapError(f)` (v3 `tapErrorCause` → `tapCause`).
- Finalizer-style: `Effect.onError((cause) => ...)`, `Effect.onErrorIf`, `Effect.onErrorFilter`,
  `Effect.onExit((exit) => ...)`, `Effect.ensuring(finalizer)`, `Effect.onInterrupt`.
- `Effect.ignoreCause` / `Effect.ignoreCause({ log: true })` — swallow everything incl. defects (best-effort work);
  `Effect.ignore` only swallows typed failures.

## sandbox

`Effect.sandbox` moves the full `Cause<E>` into the error channel (`Effect<A, Cause<E>, R>`), so ordinary error
combinators can see defects and interrupts:

```ts
import { Cause, Effect } from "effect"

const task = Effect.fail("Something went wrong")

const program = Effect.gen(function*() {
  const cause = yield* Effect.flip(Effect.sandbox(task))
  return `Caught cause: ${Cause.squash(cause)}`
})
```

`Effect.unsandbox` **was removed** in v4. To restore the original channel, fail with the cause:
`sandboxed.pipe(Effect.catch(Effect.failCause))`. Most code should use `catchCause`/`catchCauseFilter` instead of sandboxing.

## Rendering causes

- `Cause.pretty(cause)` — human-readable multi-line string (stack traces, nested errors).
- `Cause.prettyErrors(cause, { includeCauseInStack? })` — `Array<Error>` for reporters/loggers.
- `Cause.squash(cause)` — single representative value (first failure, else first defect, else interrupt).
- `Effect.logError("message", cause)` — log with the structured logger (see `observability.md`).
- Annotations: `Cause.annotations(cause)`, `Cause.reasonAnnotations(reason)`, `Cause.annotate(cause, context)`.

## ErrorReporter (new in v4)

Forward failures to monitoring/error-tracking (Sentry, logs, …). Reporters receive each non-interrupt error,
deduplicated, with `severity` and `attributes` resolved.

```ts
import { Data, Effect, ErrorReporter } from "effect"

const reporter = ErrorReporter.make(({ error, severity, attributes, cause, fiber, timestamp }) => {
  console.error(`[${severity}]`, error.message, attributes)
})

class NotFoundError extends Data.TaggedError("NotFoundError")<{}> {
  readonly [ErrorReporter.ignore] = true          // never reported (e.g. 404s)
}

class DeprecationWarning extends Data.TaggedError("DeprecationWarning")<{}> {
  readonly [ErrorReporter.severity] = "Warn" as const
  readonly [ErrorReporter.attributes] = { area: "billing" }
}

declare const handler: Effect.Effect<void, NotFoundError | DeprecationWarning>

const program = handler.pipe(
  Effect.withErrorReporting,                       // or Effect.withErrorReporting({ defectsOnly: true })
  Effect.provide(ErrorReporter.layer([reporter]))  // replaces existing; { mergeWithExisting: true } to add
)
```

- `ErrorReporter.make(callback)` → reporter; `ErrorReporter.layer([reporters], { mergeWithExisting? })`
  (reporters may also be `Effect<ErrorReporter>` resolved at layer build).
- `Effect.withErrorReporting` / `Effect.withErrorReporting({ defectsOnly: true })` — report failures of an effect
  without changing its result.
- `ErrorReporter.report(cause)` — report manually and continue.
- Annotations on error objects (`Error` is augmented with `ErrorReporter.Reportable`):
  `[ErrorReporter.ignore]`, `[ErrorReporter.severity]` (a `LogLevel.Severity`, default `"Info"`),
  `[ErrorReporter.attributes]`. Helpers: `ErrorReporter.isIgnored`, `getSeverity`, `getAttributes`.
- Built-in reporting boundaries: HTTP server (`HttpEffect`/`HttpApiBuilder`), `RpcServer`, `McpServer` report
  unhandled failures to the current reporters — provide `ErrorReporter.layer` in your app layer.
- Low level: `ErrorReporter.CurrentErrorReporters` (`Context.Reference<ReadonlySet<ErrorReporter>>`).

## v3 → v4

| v3 | v4 |
| --- | --- |
| `Effect.catchAllCause` | `Effect.catchCause` |
| `Effect.catchSomeCause` | `Effect.catchCauseFilter` / `Effect.catchCauseIf` |
| `Effect.catchAllDefect` | `Effect.catchDefect` |
| `Effect.tapErrorCause` | `Effect.tapCause` |
| `Effect.unsandbox` | `Effect.catch(Effect.failCause)` |
| `Effect.mapErrorCause` | `Effect.catchCause` + `Effect.failCause` |
| `Effect.ignoreLogged` | `Effect.ignore({ log: true })` |
| — | `ErrorReporter`, `Effect.withErrorReporting` |

## Pitfalls

- Dropping `Cause` information when converting to strings too early — keep the `Cause` until you log/report.
- Treating defects as recoverable business errors.
- Forgetting interruptions: `catchCause` also sees `Interrupt` reasons; check `Cause.hasInterruptsOnly` before reporting.
- Reporting expected errors as incidents — mark them with `[ErrorReporter.ignore] = true` or lower severity.
