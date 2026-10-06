# Error Management (Expected Errors vs Defects)

Bundled examples: `references/examples/01_effect/04_errors/01_error-handling.ts`, `10_catch-tags.ts`,
`20_reason-errors.ts`. Migration: `references/migration/error-handling.md`,
`references/migration/api-map/effect-Effect.md`.

## Mental model

- **Expected errors** live in the typed `E` channel and should be handled/translated (`Effect.fail`, `yield* new MyError()`).
- **Defects** are bugs/invariant violations (`Effect.die`, thrown exceptions in `Effect.sync`, `orDie`); untyped, normally not recovered.
- **Interruptions** are fiber cancellation. All three live in a `Cause` — see `exit-cause.md` and `error-tooling.md`.

## Defining errors

Prefer `Schema.TaggedError` (serializable, schema-validated, yieldable, `_tag` for `catchTag`):

```ts
import { Effect, Schema } from "effect"

export class ParseError extends Schema.TaggedError<ParseError>()("ParseError", {
  input: Schema.String,
  message: Schema.String          // a `message` field becomes error.message
}) {}

export class ReservedPortError extends Schema.TaggedError<ReservedPortError>()("ReservedPortError", {
  port: Schema.Int
}) {}

export class DatabaseError extends Schema.TaggedError<DatabaseError>()("DatabaseError", {
  cause: Schema.Defect()          // wrap unknown thrown values (JSON-safe)
}) {}

export class Empty extends Schema.TaggedError<Empty>()("Empty", {}) {}  // no fields: `new Empty()`
```

- `Schema.Error<Self>()("Identifier", fields)` — schema-backed error without `_tag`.
- `Data.TaggedError("Tag")<{ fields }>` / `Data.Error<{ fields }>` — lightweight, non-schema alternatives (still yieldable, still `_tag`).
- Errors are `Cause.YieldableError`: `return yield* new ParseError({...})` fails the enclosing effect.
  Always write `return yield*` so TypeScript knows the branch ends.

```ts
const loadPort = Effect.fn("loadPort")(function*(input: string): Effect.fn.Return<number, ParseError | ReservedPortError> {
  const port = Number(input)
  if (!Number.isInteger(port)) return yield* new ParseError({ input, message: "not an integer" })
  if (port < 1024) return yield* new ReservedPortError({ port })
  return port
})
```

## Recovering

```ts
// one tag, or several tags at once (array form)
loadPort("80").pipe(
  Effect.catchTag(["ParseError", "ReservedPortError"], (_) => Effect.succeed(3000))
)

loadPort("invalid").pipe(
  Effect.catchTag("ReservedPortError", (_) => Effect.succeed(3000)),
  Effect.catch((_) => Effect.succeed(3000))          // everything else (v3: catchAll)
)

// object of handlers
loadPort("x").pipe(
  Effect.catchTags({
    ParseError: (e) => Effect.succeed(`bad input: ${e.input}`),
    ReservedPortError: (e) => Effect.succeed(`reserved: ${e.port}`)
  })
)
```

- `Effect.catchTag(tag | [tags], handler, orElse?)` — optional `orElse` handles the remaining errors.
- `Effect.catchTags({ ... }, orElse?)` — same, keyed by `_tag`.
- `Effect.catchIf(predicateOrRefinement, handler, orElse?)` — recover errors matching a predicate.
- `Effect.catchFilter(filter, handler, orElse?)` — `Filter`-based (v3 `catchSome`), e.g.
  `Effect.catchFilter(Filter.fromPredicate((e: number) => e === 42), () => Effect.succeed("caught"))`.
- `Effect.catchNoSuchElement` — turns `Cause.NoSuchElementError` into `Option.none` (success becomes `Option`).
- `Effect.catchEager(handler)` — like `catch` but evaluates synchronous recovery effects eagerly (perf optimization).
- `Effect.orElseSucceed(() => value)` — replace any typed error with a success value.
- `Effect.catchDefect`, `Effect.catchCause`, `Effect.catchCauseIf`, `Effect.catchCauseFilter` — see `error-tooling.md`.

## Errors with a `reason` (nested causes)

Model a family of failures as one tagged error with a tagged `reason` union; handle reasons without unwrapping
(`AiError` and `SqlError` in `effect/ai` / `effect/sql` use this pattern):

```ts
import { Effect, Schema } from "effect"

export class RateLimitError extends Schema.TaggedError<RateLimitError>()("RateLimitError", { retryAfter: Schema.Finite }) {}
export class QuotaExceededError extends Schema.TaggedError<QuotaExceededError>()("QuotaExceededError", { limit: Schema.Int }) {}
export class SafetyBlockedError extends Schema.TaggedError<SafetyBlockedError>()("SafetyBlockedError", { category: Schema.String }) {}

export class AiError extends Schema.TaggedError<AiError>()("AiError", {
  reason: Schema.Union([RateLimitError, QuotaExceededError, SafetyBlockedError])
}) {}

declare const callModel: Effect.Effect<string, AiError>

// one reason (+ optional catch-all for other reasons)
callModel.pipe(
  Effect.catchReason(
    "AiError",
    "RateLimitError",
    (reason) => Effect.succeed(`Retry after ${reason.retryAfter} seconds`),
    (reason) => Effect.succeed(`Model call failed for reason: ${reason._tag}`)
  )
)

// several reasons
callModel.pipe(
  Effect.catchReasons("AiError", {
    RateLimitError: (reason) => Effect.succeed(`Retry after ${reason.retryAfter} seconds`),
    QuotaExceededError: (reason) => Effect.succeed(`Quota exceeded at ${reason.limit} tokens`)
  })
)

// move reasons into the error channel, then use normal combinators
callModel.pipe(
  Effect.unwrapReason("AiError"),
  Effect.catchTags({
    RateLimitError: (r) => Effect.succeed(`Back off for ${r.retryAfter} seconds`),
    QuotaExceededError: (r) => Effect.succeed(`Increase quota beyond ${r.limit}`),
    SafetyBlockedError: (r) => Effect.succeed(`Blocked by safety category: ${r.category}`)
  })
)
```

Handlers receive `(reason, parentError)`. Unhandled reasons stay in the error channel as the parent error.

## Transforming and observing

- `Effect.mapError(f)`, `Effect.mapBoth({ onFailure, onSuccess })`.
- `Effect.orDie` — typed error → defect. v3 `orDieWith(f)` → `Effect.mapError(f)` + `Effect.orDie`.
- `Effect.tapError(f)`, `Effect.tapErrorTag(tag | [tags], f)`, `Effect.tapCause`, `Effect.tapDefect`.
- `Effect.result` (→ `Result<A, E>`), `Effect.option`, `Effect.exit`, `Effect.match({ onFailure, onSuccess })`,
  `Effect.matchEffect`, `Effect.flip`.
- `Effect.filterOrFail(pred, (a) => err)` — v3 `filterOrDie` → `filterOrFail` + `orDie`.
- `Effect.ignore` / `Effect.ignore({ log: true })` — discard failures (v3 `ignoreLogged` → `ignore({ log: true })`).
- `Effect.retry(schedule | options)`, `Effect.retryOrElse` — see `scheduling-retry.md`.
- Built-in error types (in `Cause`): `NoSuchElementError`, `TimeoutError`, `IllegalArgumentError`,
  `ExceededCapacityError`, `UnknownError` (from thunk `Effect.try`/`tryPromise`) — all have `_tag`s usable with `catchTag`.

## Boundaries

```ts
import { Effect, Schema } from "effect"

class UserLookupError extends Schema.TaggedError<UserLookupError>()("UserLookupError", {
  userId: Schema.Int,
  cause: Schema.Defect()
}) {}

export const fetchUser = Effect.fn("fetchUser")((userId: number) =>
  Effect.tryPromise({
    try: (signal) => fetch(`/users/${userId}`, { signal }).then((r) => r.json()),
    catch: (cause) => new UserLookupError({ userId, cause })
  })
)
```

Translate low-level errors into domain errors at module boundaries; keep `unknown` only inside a `cause` field.

## v3 → v4

| v3 | v4 |
| --- | --- |
| `Effect.catchAll` | `Effect.catch` |
| `Effect.catchAllCause` / `catchAllDefect` | `Effect.catchCause` / `Effect.catchDefect` |
| `Effect.catchSome` / `catchSomeCause` | `Effect.catchFilter` / `Effect.catchCauseFilter` |
| `Effect.catchSomeDefect` | removed — `catchDefect` + re-`die` |
| `Effect.catchTag("A")` then `catchTag("B")` | also `Effect.catchTag(["A", "B"], ...)` |
| — | `Effect.catchReason`, `catchReasons`, `unwrapReason`, `catchEager`, `catchNoSuchElement` |
| `Effect.either` | `Effect.result` |
| `Effect.orElse` / `orElseFail` | `Effect.catch` / `Effect.mapError` |
| `Effect.orDieWith` / `dieMessage` | `mapError` + `orDie` / `Effect.die(new Error(msg))` |
| `Effect.tapErrorCause` | `Effect.tapCause` |
| `Cause.NoSuchElementException` etc. | `Cause.NoSuchElementError` etc. |
| `Schema.TaggedError<Self>()("Tag", {...})` | unchanged API, v4 Schema implementation |

## Pitfalls

- `throw` inside `Effect.gen` / `Effect.sync` → defect, not a typed error.
- Forgetting `return` on `yield* new MyError(...)`.
- Collapsing errors to `unknown`/`string` early — keep tagged unions until the boundary.
- Catching defects as business logic; use `catchDefect` only at integration/reporting boundaries.
