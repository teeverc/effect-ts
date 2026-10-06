# Generators (Effect.gen, Effect.fn, yield*)

Bundled examples: `references/examples/01_effect/01_basics/01_effect-gen.ts`, `02_effect-fn.ts`.
Migration notes: `references/migration/generators.md`, `references/migration/yieldable.md`.

## Mental model

- `Effect.gen(function*() { ... })` is async/await for Effects: `yield*` runs an Effect and returns its success value.
- A typed failure short-circuits the generator (like a thrown error); the error type is the union of everything yielded.
- Requirements (`R`) are the union of all yielded effects' requirements.
- `Effect.gen` builds a lazy description — nothing runs until the effect is run.

## Which form to use

| Situation | Use |
| --- | --- |
| inline workflow / layer body / one-off program | `Effect.gen(function*() { ... }).pipe(...)` |
| reusable function returning an Effect, worth a span | `Effect.fn("Module.name")(function*(args) { ... }, ...transforms)` |
| reusable function in library code / hot path | `Effect.fnUntraced(function*(args) { ... })` |
| generic stack-frame boundary without a span | `Effect.fn(function*(args) { ... })` |

Do **not** write `const f = (x: X) => Effect.gen(function*() { ... })` — `Effect.fn` reuses the generator body
instead of allocating a closure per call, and adds tracing.

## Effect.gen

```ts
import { Effect, Schema } from "effect"

export class FileProcessingError extends Schema.TaggedError<FileProcessingError>()("FileProcessingError", {
  message: Schema.String
}) {}

Effect.gen(function*() {
  yield* Effect.log("Reading file...")
  // Always `return yield*` when failing so TS knows execution stops here.
  return yield* new FileProcessingError({ message: "Failed to read the file" })
}).pipe(
  Effect.catch((error) => Effect.logError(`An error occurred: ${error}`)),
  Effect.withSpan("fileProcessing", { attributes: { method: "Effect.gen" } })
)
```

Binding `this` (v4 changed the signature — v3 `Effect.gen(this, fn)`):

```ts
class MyService {
  readonly local = 1
  compute = Effect.gen({ self: this }, function*() {
    return yield* Effect.succeed(this.local + 1)
  })
}
```

## Effect.fn / Effect.fnUntraced

```ts
import { Effect, Schema } from "effect"

class SomeError extends Schema.TaggedError<SomeError>()("SomeError", { message: Schema.String }) {}

export const effectFunction = Effect.fn("effectFunction")(
  function*(n: number): Effect.fn.Return<string, SomeError> {
    yield* Effect.logInfo("Received number:", n)
    if (n < 0) return yield* new SomeError({ message: "negative" })
    return `ok ${n}`
  },
  // Extra args act like pipe transforms; each receives (effect, ...originalArgs).
  // Do NOT call .pipe on the result of Effect.fn.
  Effect.catch((error) => Effect.logError(`An error occurred: ${error}`)),
  Effect.annotateLogs({ method: "effectFunction" })
)

// Transform with access to the original arguments
const formatLength = Effect.fn("formatLength")(
  function*(value: string) {
    return yield* Effect.succeed(value.length)
  },
  (effect, value) => effect.pipe(Effect.map((length) => `${value}: ${length}`))
)

// No span / stack-frame capture
export const validateBatchSize = Effect.fnUntraced(function*(size: number): Effect.fn.Return<number, SomeError> {
  if (!Number.isInteger(size) || size <= 0) {
    return yield* new SomeError({ message: "Batch size must be a positive integer" })
  }
  return size
})
```

- `Effect.fn("name", spanOptions?)` — options are `SpanOptionsNoTrace` (attributes, links, parent/root, kind, sampling, log level).
- `Effect.fn.Return<A, E = never, R = never>` annotates the generator return; works with generics:
  `function*<A>(value: A): Effect.fn.Return<A> { ... }`. (v3 `Effect.fn.Gen` → `Effect.fn.Return`.)
- Bind `this`: `Effect.fn("Counter.increment")({ self: this }, function*(this: Counter, by: number) { ... })`.
- Service methods: define them with `Effect.fn("Service.method")` inside the layer's `Effect.gen` (see
  `dependency-management.md`).
- `Effect.fnUntracedEager` is an optimization that runs synchronous generator steps eagerly; rarely needed.

## What can be yielded (v4)

`yield*` works on `Effect` values and Effect subtypes:

- Any `Effect`.
- Errors created with `Schema.TaggedError`, `Schema.Error`, `Data.TaggedError`, `Data.Error`
  (`Cause.YieldableError`) — yielding fails the effect.
- Service classes (`Context.Service`) and `Context.Reference`s — yield to access the service.
- `Config` values — yield to load from the current `ConfigProvider`.
- Custom `Effectable` values (below).

**Not** yieldable in `Effect.gen` (v3 subtypes removed — there is no `Yieldable` trait and no `.asEffect()` on these):

| Value | v4 |
| --- | --- |
| `Option<A>` | `yield* Effect.fromOption(opt)` (fails with `Cause.NoSuchElementError`) or `Effect.fromOption(opt, () => err)` |
| `Result<A, E>` | `yield* Effect.fromResult(result)` |
| `Ref<A>` | `yield* Ref.get(ref)` |
| `Deferred<A, E>` | `yield* Deferred.await(deferred)` |
| `Fiber<A, E>` | `yield* Fiber.join(fiber)` |

`Option.gen` / `Result.gen` exist for pure Option/Result generators.

```ts
import { Config, Context, Deferred, Effect, Fiber, Option, Ref } from "effect"

class Greeting extends Context.Service<Greeting, { readonly message: string }>()("app/Greeting") {}

const program = Effect.gen(function*() {
  const greeting = yield* Greeting
  const name = yield* Config.String("NAME").pipe(Config.withDefault("world"))
  const n = yield* Effect.fromOption(Option.some(41))

  const ref = yield* Ref.make(0)
  yield* Ref.update(ref, (x) => x + n)
  const count = yield* Ref.get(ref)

  const deferred = yield* Deferred.make<string>()
  yield* Deferred.succeed(deferred, "done")
  const value = yield* Deferred.await(deferred)

  const fiber = yield* Effect.forkChild(Effect.succeed(1))
  const joined = yield* Fiber.join(fiber)

  return `${greeting.message}, ${name}! ${count} ${value} ${joined}`
}).pipe(Effect.provideService(Greeting, { message: "Hello" }))
```

## Custom Effect-like values: `Effectable`

`Effectable.Class` is abstract; implement `asEffect()`:

```ts
import { DateTime, Effect, Effectable } from "effect"

class CurrentInstant extends Effectable.Class<DateTime.Utc> {
  asEffect() {
    return DateTime.now
  }
}

const program = Effect.gen(function*() {
  return yield* new CurrentInstant()
})
```

- `Effectable.Mixin(ExistingClass)` adds Effect behaviour to an existing class (implement `asEffect()` on the subclass).
- `Effectable.Prototype({ label, evaluate(fiber) })` is the low-level prototype builder.
- Use these sparingly — ordinary data should expose explicit functions returning `Effect`.

## Pitfalls

- Forgetting `return` before `yield* new MyError(...)` (TS thinks code continues; types widen).
- `throw` inside a generator → becomes a defect, not a typed error. Use `return yield* new MyError(...)`.
- `.pipe(...)` on an `Effect.fn` function — pass transforms as extra arguments instead.
- Wrapping `Effect.gen` in arrow functions instead of `Effect.fn`.
- Yielding `Option`, `Result`, `Ref`, `Deferred`, `Fiber` directly — convert explicitly.
- Recommending removed `Effect.Yieldable`, `Effect.fn.Gen`, or v3 `Effect.gen(this, ...)`.
