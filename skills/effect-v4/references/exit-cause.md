# Exit and Cause (flat Cause, Reason, Exit)

Use when you need the full outcome of an effect: diagnostics, reporting, tests, framework boundaries.
Migration details: `references/migration/cause.md`, `references/migration/api-map/effect-Cause.md`,
`references/migration/api-map/effect-Exit.md`. Handling combinators: `error-tooling.md`.

## Cause: a flat list of reasons

```ts
interface Cause<E> {
  readonly reasons: ReadonlyArray<Reason<E>>
}
type Reason<E> = Fail<E> | Die | Interrupt

interface Fail<E>  { readonly _tag: "Fail";      readonly error: E }
interface Die      { readonly _tag: "Die";       readonly defect: unknown }
interface Interrupt { readonly _tag: "Interrupt"; readonly fiberId: number | undefined }
```

- v3's recursive tree (`Empty | Fail | Die | Interrupt | Sequential | Parallel`) is gone. An empty cause has
  `reasons.length === 0`; sequential/parallel failures are concatenated (`Cause.combine`).
- Fiber ids are plain `number`s (no `FiberId` module).
- `Cause` implements `Equal`, `Pipeable`, `Inspectable`.

### Constructors

`Cause.empty`, `Cause.fail(e)`, `Cause.die(defect)`, `Cause.interrupt(fiberId?)`, `Cause.fromReasons([...])`,
`Cause.combine(a, b)` (v3 `sequential`/`parallel`), `Cause.makeFailReason(e)`, `Cause.makeDieReason(d)`,
`Cause.makeInterruptReason(fiberId?)`, `Cause.map(cause, f)`.

### Inspecting

| Purpose | API |
| --- | --- |
| guards | `Cause.isCause(u)`, `Cause.isReason(u)`, `Cause.isFailReason(r)`, `Cause.isDieReason(r)`, `Cause.isInterruptReason(r)` |
| presence | `Cause.hasFails`, `Cause.hasDies`, `Cause.hasInterrupts`, `Cause.hasInterruptsOnly` |
| first typed error | `Cause.findError(c)` → `Result<E, Cause<never>>`, `Cause.findErrorOption(c)` → `Option<E>`, `Cause.findFail(c)` → `Result<Fail<E>, ...>` |
| first defect | `Cause.findDefect(c)` → `Result<unknown, Cause<E>>`, `Cause.findDie(c)` → `Result<Die, ...>` |
| interrupts | `Cause.findInterrupt(c)`, `Cause.interruptors(c)` → `ReadonlySet<number>`, `Cause.filterInterruptors(c)` |
| all of a kind | `cause.reasons.filter(Cause.isFailReason)` (v3 `Cause.failures`), `.filter(Cause.isDieReason)` (v3 `defects`) |
| render | `Cause.pretty(c)`, `Cause.prettyErrors(c)`, `Cause.squash(c)` (lossy: first error, else defect, else interrupt) |
| annotations | `Cause.annotations(c)`, `Cause.reasonAnnotations(r)`, `Cause.annotate(c, ctx)`, `Cause.StackTrace` |

`find*` functions return `Result` (not `Option`) so they double as `Filter`s for
`Effect.catchCauseFilter` / `Effect.tapCauseFilter`.

```ts
import { Cause } from "effect"

const describe = (cause: Cause.Cause<string>): Array<string> =>
  cause.reasons.map((reason) => {
    switch (reason._tag) {
      case "Fail":
        return `fail: ${reason.error}`
      case "Die":
        return `die: ${String(reason.defect)}`
      case "Interrupt":
        return `interrupted by ${reason.fiberId ?? "unknown"}`
    }
  })
```

### Built-in error classes (in `Cause`)

`NoSuchElementError`, `TimeoutError`, `IllegalArgumentError`, `ExceededCapacityError`, `AsyncFiberError`,
`UnknownError` (each with an `is*` guard and a `_tag`), plus `Cause.Done` / `Cause.done()` / `Cause.isDone` — a
graceful completion signal used by queues, streams and `Pull`. `Cause.YieldableError` is the base interface for
errors that can be `yield*`-ed. v3 `*Exception` names → `*Error`; `RuntimeException` / `InterruptedException` removed.

## Exit

`Exit<A, E> = Success<A, E> { value: A } | Failure<A, E> { cause: Cause<E> }`. In v4 `Exit` is itself an `Effect`
subtype (yield it or pass it to combinators).

```ts
import { Cause, Effect, Exit } from "effect"

const program = Effect.fail("boom").pipe(
  Effect.exit,
  Effect.map((exit) =>
    Exit.match(exit, {
      onSuccess: (value) => `ok: ${value}`,
      onFailure: (cause) => Cause.pretty(cause)
    })
  )
)

const exit = await Effect.runPromiseExit(Effect.succeed(1))
if (Exit.isSuccess(exit)) exit.value
else exit.cause
```

- Constructors: `Exit.succeed`, `Exit.fail`, `Exit.die`, `Exit.failCause`, `Exit.interrupt(fiberId?)`, `Exit.void`.
- Guards: `Exit.isExit`, `isSuccess`, `isFailure`, `hasFails`, `hasDies`, `hasInterrupts` (v3 `isInterrupted`).
- Extract: `Exit.getSuccess` → `Option<A>`, `Exit.getCause` → `Option<Cause<E>>` (v3 `causeOption`),
  `Exit.findError` / `findDefect` → `Result`, `Exit.findErrorOption`, `filterSuccess`, `filterValue`,
  `filterFailure`, `filterCause` (Result-returning, usable as Filters).
- Transform: `Exit.map`, `mapError`, `mapBoth({ onFailure, onSuccess })`, `asVoid`, `Exit.asVoidAll(exits)`
  (combines failures; replaces v3 `Exit.all`/`zip*`).
- Producing exits: `Effect.exit`, `Effect.runPromiseExit`, `Effect.runSyncExit`, `Fiber.await`, `Effect.onExit`.

## Exit vs Result

- `Result<A, E>` (via `Effect.result`) — typed success/failure only; defects/interrupts still propagate. Use for
  business branching.
- `Exit<A, E>` (via `Effect.exit`) — everything, including defects and interruption. Use for boundaries,
  supervisors, tests and reporting.

## v3 → v4

| v3 | v4 |
| --- | --- |
| `switch (cause._tag)` over `Sequential`/`Parallel`/`Empty` | iterate `cause.reasons` |
| `Cause.isFailure` / `isDie` / `isInterrupted` / `isInterruptedOnly` | `Cause.hasFails` / `hasDies` / `hasInterrupts` / `hasInterruptsOnly` |
| `Cause.isFailType` / `isDieType` / `isInterruptType` | `Cause.isFailReason` / `isDieReason` / `isInterruptReason` (on a reason) |
| `Cause.failureOption` / `failureOrCause` | `Cause.findErrorOption` / `Cause.findError` |
| `Cause.dieOption` / `interruptOption` | `Cause.findDefect` / `Cause.findInterrupt` (Result) |
| `Cause.failures` / `defects` | `cause.reasons.filter(Cause.isFailReason / isDieReason)` |
| `Cause.sequential` / `parallel` | `Cause.combine` |
| `Cause.NoSuchElementException` / `TimeoutException` / `UnknownException` | `Cause.NoSuchElementError` / `TimeoutError` / `UnknownError` |
| `Exit.isInterrupted` / `causeOption` | `Exit.hasInterrupts` / `Exit.getCause` |
| `Exit.all` / `Exit.zip*` | `Exit.asVoidAll` (+ `Exit.match`) |
| `FiberId` objects | `number` fiber ids |

## Pitfalls

- Using v3 tree-matching on `Cause`.
- Treating `Cause.squash` as complete — it keeps only one failure; iterate `reasons` or use `Cause.prettyErrors`.
- Reporting interrupt-only causes as errors (check `Cause.hasInterruptsOnly`).
- Using `Exit` for business logic where `Result` suffices.
