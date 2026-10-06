# Core Usage (Effect, Option, Result, Predicate, Collections, Match)

Everyday Effect 4.0.x composition. Imports are from the `effect` barrel
(`import { Effect, Option } from "effect"`) or per module (`import * as Effect from "effect/Effect"`).

Bundled examples: `references/examples/01_effect/01_basics/` (`01_effect-gen.ts`,
`02_effect-fn.ts`, `10_creating-effects.ts`), `references/examples/10_predicate/01_basics.ts`.

## Style rules (from upstream LLMS.md)

- Inline workflows: `Effect.gen(function*() { ... })`, then attach behaviour with `.pipe(...)`.
- Reusable functions returning an Effect: `Effect.fn("name")(function*(...) { ... }, ...combinators)`.
  The name should match the function name; it creates a span and better stack traces.
- Library code / hot paths: `Effect.fnUntraced(function*(...) { ... })` (no span, no stack-frame capture).
- **Never** write `const f = (x) => Effect.gen(...)` wrappers — use `Effect.fn` / `Effect.fnUntraced`.
- **Never** `.pipe` an `Effect.fn` result; pass combinators as extra arguments to `Effect.fn`.
- Annotate return types with `Effect.fn.Return<A, E, R>`.
- Fail with `return yield* new MyError(...)` (always `return`, so TS knows control stops).
- Define errors with `Schema.TaggedError` (see `error-management.md`); parse untrusted data with
  `Schema`, not hand-written predicates; use `Predicate` helpers for small runtime guards;
  use `DateTime` instead of `Date` / `Date.now()`.

```ts
import { Effect, Schema } from "effect"

export class SomeError extends Schema.TaggedError<SomeError>()("SomeError", {
  message: Schema.String
}) {}

export const effectFunction = Effect.fn("effectFunction")(
  function*(n: number): Effect.fn.Return<string, SomeError> {
    yield* Effect.logInfo("Received number:", n)
    return yield* new SomeError({ message: "Failed to read the file" })
  },
  // extra args act like pipe transforms (do NOT use .pipe on Effect.fn)
  Effect.catch((error) => Effect.logError(`An error occurred: ${error}`)),
  Effect.annotateLogs({ method: "effectFunction" })
)

export const validateBatchSize = Effect.fnUntraced(function*(size: number): Effect.fn.Return<number, SomeError> {
  if (!Number.isInteger(size) || size <= 0) {
    return yield* new SomeError({ message: "Batch size must be a positive integer" })
  }
  return size
})
```

See `generators.md` for `Effect.gen` / `Effect.fn` details.

## Creating effects

| Source | Constructor |
| --- | --- |
| value | `Effect.succeed(a)`, `Effect.void`, `Effect.succeedNone`, `Effect.succeedSome(a)` |
| typed failure / defect | `Effect.fail(e)`, `Effect.die(defect)`, `Effect.failCause(cause)` |
| sync side effect (won't throw) | `Effect.sync(() => ...)` |
| sync code that may throw | `Effect.try({ try, catch })` (thunk form fails with `Cause.UnknownError`) |
| Promise that never rejects | `Effect.promise((signal) => ...)` |
| Promise that may reject | `Effect.tryPromise({ try: (signal) => ..., catch })` (thunk form → `Cause.UnknownError`) |
| callback API | `Effect.callback<A, E>((resume) => { ...; return Effect.sync(cleanup) })` (v3 `Effect.async`) |
| nullable value | `Effect.fromNullishOr(v)` → fails with `Cause.NoSuchElementError` |
| `Option` | `Effect.fromOption(opt)` or `Effect.fromOption(opt, () => new MyError())` |
| `Result` | `Effect.fromResult(result)` |
| lazy construction | `Effect.suspend(() => effect)` |

`Option` and `Result` are **not** Effects in v4 — convert with `Effect.fromOption` / `Effect.fromResult`
before yielding (see `references/migration/yieldable.md`).

```ts
import { Effect, Schema } from "effect"

class InvalidPayload extends Schema.TaggedError<InvalidPayload>()("InvalidPayload", {
  input: Schema.String,
  cause: Schema.Defect()
}) {}
class MissingWorkspaceId extends Schema.TaggedError<MissingWorkspaceId>()("MissingWorkspaceId", {}) {}

export const parsePayload = Effect.fn("parsePayload")((input: string) =>
  Effect.try({
    try: () => JSON.parse(input) as { readonly userId: number },
    catch: (cause) => new InvalidPayload({ input, cause })
  })
)

declare const headers: Map<string, string>
export const workspaceId = Effect.fromNullishOr(headers.get("x-workspace-id")).pipe(
  Effect.mapError(() => new MissingWorkspaceId())
)
```

## Common combinators

- Transform / sequence: `Effect.map`, `Effect.flatMap`, `Effect.andThen` (v3 `zipRight`), `Effect.tap`,
  `Effect.as`, `Effect.asVoid`, `Effect.zip`, `Effect.zipWith`, `Effect.flatten`.
- Collections: `Effect.all([...] | {...}, { concurrency, mode: "result", discard })`,
  `Effect.forEach(items, f, { concurrency, discard })`, `Effect.validate`, `Effect.partition`,
  `Effect.reduce`, `Effect.findFirst`.
- Branching: `Effect.match({ onFailure, onSuccess })`, `Effect.matchEffect`, `Effect.matchCause`,
  `Effect.when(self, conditionEffect)` (returns `Option<A>`),
  `Effect.filterOrFail(predicate, orFailWith?)`, `Effect.filterOrElse`.
- Materialize: `Effect.result` (v3 `Effect.either`) → `Result<A, E>`, `Effect.option`, `Effect.exit`.
- Errors: `Effect.catch` (v3 `catchAll`), `Effect.catchTag`, `Effect.mapError`, `Effect.orDie` — see `error-management.md`.
- Time: `Effect.sleep("1 second")`, `Effect.timeout(d)` (fails with `Cause.TimeoutError`),
  `Effect.timeoutOption`, `Effect.delay`, `Effect.timed`.
- Alternatives: `Effect.firstSuccessOf([a, b, c])`, `Effect.orElseSucceed(() => fallback)`.
- Interop: `Effect.abortSignal` (scoped `AbortSignal` tied to fiber interruption),
  `Effect.acquireDisposable` for `Disposable` / `AsyncDisposable` resources.
- Run at the edge only: `Effect.runPromise`, `Effect.runPromiseExit`, `Effect.runFork`, `Effect.runSync`;
  for apps use `NodeRuntime.runMain` / `Layer.launch` (see `runtime-execution.md`).

## Option

```ts
import { Option } from "effect"

const port = Option.fromNullishOr(process.env.PORT).pipe(   // v3: fromNullable
  Option.map(Number),
  Option.filter((n) => n > 0),
  Option.getOrElse(() => 3000)
)

Option.match(Option.some(1), { onNone: () => "none", onSome: (n) => `some ${n}` })
Option.isSome(o) && o.value      // narrowing
Option.getOrUndefined(o); Option.getOrNull(o); Option.getOrThrow(o)
Option.gen(function*() { const a = yield* Option.some(1); return a + 1 }) // Option-only generator
```

Renames vs v3: `fromNullable`→`fromNullishOr`, `liftNullable`→`liftNullishOr`,
`flatMapNullable`→`flatMapNullishOr`, `getEquivalence`→`makeEquivalence`, `getOrder`→`makeOrder`,
`getRight/getLeft`→`getSuccess/getFailure`. `Option.filterMap` takes a `Filter` (Result-returning
function). See `references/migration/api-map/effect-Option.md`.

## Result (replaces v3 `Either`)

`Result<A, E>` = `Success { success: A }` | `Failure { failure: E }`. Note argument order: success first.

```ts
import { Result } from "effect"

const parsed = Result.try({ try: () => JSON.parse(raw), catch: (e) => `bad json: ${e}` })
const n = Result.succeed(1).pipe(Result.map((x) => x + 1), Result.getOrElse(() => 0))
Result.match(parsed, { onFailure: (e) => e, onSuccess: (v) => String(v) })
if (Result.isFailure(parsed)) parsed.failure
Result.fromOption(opt, () => "missing")
Result.gen(function*() { const a = yield* Result.succeed(1); return a })
```

Inside Effect code: `Effect.result(eff)` gives `Effect<Result<A, E>>`; `Effect.fromResult(r)` converts back.
v3 `Either.right/left` → `Result.succeed/fail`; `Effect.either` → `Effect.result`. See
`references/migration/api-map/effect-Either.md`.

## Predicate (runtime guards)

Never hand-roll `isRecord` / `isString`. Use `Predicate`:

```ts
import { Predicate } from "effect"

const thing: unknown = { a: 1 }
if (Predicate.isObject(thing) && Predicate.isNumber(thing.a)) {
  console.log("number", thing.a)
}
```

Guards: `isString`, `isNumber`, `isBoolean`, `isBigInt`, `isSymbol`, `isFunction`, `isUndefined`,
`isNull`, `isNullish`, `isNotNullish`, `isObject` (non-null, non-array object), `isReadonlyObject`,
`isObjectOrArray`, `isObjectKeyword` (any `typeof === "object"`/function, non-null), `isDate`, `isRegExp`,
`isError`, `isIterable`, `isPromise`, `isPromiseLike`, `isSet`, `isMap`, `isTupleOf(n)`,
`hasProperty(u, "key")`, `isTagged(u, "Tag")`.
Combinators: `Predicate.and`, `or`, `not`, `xor`, `compose`, `mapInput`, `every`, `some`, `Predicate.Struct`, `Predicate.Tuple`.
For untrusted structured input use `Schema` decoding instead (see `schema.md`).

## Array / Record / Chunk

`Array` (`import { Array } from "effect"`, shadows the global — alias it if needed) works on any `Iterable`
and returns new arrays.

- Basics: `Array.map`, `filter`, `flatMap`, `reduce`, `findFirst` (→ `Option`), `head`, `last`, `get`,
  `groupBy` (→ record of non-empty arrays), `dedupe` (uses `Equal`), `sort(order)`, `sortBy(...orders)`,
  `chunksOf`, `zip`, `union`, `intersection`, `difference`, `countBy`, `range`, `makeBy`, `replicate`.
- Result-based: `Array.filterMap(xs, (x, i) => Result)`, `getSuccesses`, `getFailures`, `getSomes`.
- **Changed in 4.0.0 stable**: `Array.partition`, `Array.separate`, `Chunk.partition`, `Chunk.separate`,
  `Record.partition`, `Record.separate`, `Effect.partition` and `Option.partitionMap` now return
  **`[passes/successes, fails/failures]`** (previously `[failures, successes]`). Swap destructuring when porting.
  `Array.partition` takes a `Result`-returning function, not a boolean predicate.

```ts
import { Array, Result } from "effect"

const [passes, fails] = Array.partition([1, -2, 3], (n, i) =>
  n > 0 ? Result.succeed(n + i) : Result.fail(`negative:${n}`)
) // [[1, 5], ["negative:-2"]]

const [ok, errs] = Array.separate([Result.succeed(1), Result.fail("x")]) // [[1], ["x"]]
```

- `Record`: `map`, `filter`, `filterMap`, `collect`, `keys`, `values`, `toEntries`, `fromEntries`,
  `get` (→ `Option`), `has`, `set`, `remove`, `union`, `partition`.
- `Chunk`: immutable indexed sequence used by Stream: `Chunk.make`, `fromIterable`, `append`, `appendAll`,
  `map`, `filter`, `toArray`, `toReadonlyArray`, `size`, `isEmpty`.
- `Struct`: `pick`, `omit`, `evolve`, `renameKeys`, `get`, `keys`.

## Function / pipe

```ts
import { Function, pipe } from "effect"   // or: import { pipe, flow, identity } from "effect/Function"

pipe(5, (n) => n * 2, String)        // "10"
const f = Function.flow((n: number) => n * 2, String)
```

Every Effect data type also has a `.pipe(...)` method — prefer `value.pipe(...)`. Helpers:
`identity`, `constant`, `constVoid`, `constTrue`, `absurd`, `dual` (data-first + data-last APIs),
`flip`, `memoize`, `satisfies`, `cast`.

## Match

```ts
import { Match } from "effect"

const describe = Match.type<string | number>().pipe(
  Match.when(Match.number, (n) => `number: ${n}`),
  Match.when(Match.string, (s) => `string: ${s}`),
  Match.exhaustive
)

type Event = { readonly _tag: "Created"; readonly id: string } | { readonly _tag: "Deleted"; readonly id: string }
declare const event: Event

Match.valueTags(event, {
  Created: (e) => `created ${e.id}`,
  Deleted: (e) => `deleted ${e.id}`
})

Match.value(event).pipe(
  Match.tag("Created", (e) => e.id),
  Match.orElse(() => "other")
)
```

Also: `Match.tags({...})`, `Match.tagsExhaustive({...})`, `Match.discriminator("type")(...)`,
`Match.whenOr`, `Match.whenAnd`, `Match.not`, `Match.option`, `Match.result`, `Match.orElseAbsurd`.

## v3 → v4 quick reference

| v3 | v4 |
| --- | --- |
| `Either` / `Effect.either` | `Result` / `Effect.result` |
| `Effect.async` | `Effect.callback` |
| `Effect.catchAll` / `catchAllCause` | `Effect.catch` / `Effect.catchCause` |
| `Effect.zipRight` | `Effect.andThen` |
| `Effect.fork` | `Effect.forkChild` |
| `Option.fromNullable` | `Option.fromNullishOr` |
| `yield* someOption` in `Effect.gen` | `yield* Effect.fromOption(someOption)` |
| partition → `[fails, passes]` | partition → `[passes, fails]` (4.0.0 stable) |

Full per-module maps: `references/migration/api-map/effect-Effect.md`, `effect-Array.md`,
`effect-Option.md`, `effect-Either.md`.
