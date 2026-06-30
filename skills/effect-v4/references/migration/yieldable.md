# Effect Subtyping (v3) -> Effectable / Explicit Access (v4)

In v3, many values were structural subtypes of `Effect`. This included types
such as `Ref`, `Deferred`, `Fiber`, `FiberRef`, `Option`, `Either`, and service
tags. That was convenient, but it meant ordinary values could be silently
passed to Effect combinators and interpreted as workflows.

Current v4 betas remove the broad subtyping model. Do not recommend
`Effect.Yieldable` or `.asEffect()`: `Effect.Yieldable` was removed upstream in
May 2026. Use explicit module functions for ordinary handles, and use the
`Effectable` module only when defining a custom value that truly should behave
as an `Effect`.

## Quick Mapping

| v3 pattern | v4 pattern |
| --- | --- |
| `yield* ref` | `yield* Ref.get(ref)` |
| `yield* deferred` | `yield* Deferred.await(deferred)` |
| `yield* fiber` | `yield* Fiber.join(fiber)` |
| `yield* fiberRef` | `yield* Effect.service(ref)` or `Context.Reference`-based access |
| `Option` / `Either` as an `Effect` | convert explicitly with module APIs or branch before entering Effect combinators |
| Custom value extends `Effect` structurally | extend `Effectable.Class` or use `Effectable.Prototype` |

`Config` remains Effect-like in current v4 and can be yielded directly to load
from the current `ConfigProvider`.

## Handles Are Plain Values

```ts
import { Deferred, Effect, Fiber, Ref } from "effect"

const program = Effect.gen(function*() {
  const ref = yield* Ref.make(0)
  const value = yield* Ref.get(ref)

  const deferred = yield* Deferred.make<string, never>()
  const fiber = yield* Effect.forkChild(Deferred.await(deferred))

  yield* Deferred.succeed(deferred, "done")
  const result = yield* Fiber.join(fiber)

  return { value, result }
})
```

## Services Still Yield Ergonomically

`Context.Service` and `Context.Reference` service keys are Effect-like values.
It is still idiomatic to yield service keys directly inside `Effect.gen`.

```ts
import { Context, Effect } from "effect"

class Logger extends Context.Service<Logger, {
  readonly log: (message: string) => Effect.Effect<void>
}>()("Logger") {}

const program = Effect.gen(function*() {
  const logger = yield* Logger
  yield* logger.log("ready")
})
```

## Custom Effect-like Values

Use `Effectable.Class` for class-based custom effects, or
`Effectable.Prototype` for lower-level prototype construction. Prefer ordinary
functions returning `Effect` unless the value itself is intended to be executed
by the Effect runtime.

```ts
import { Effect, Effectable } from "effect"

class Now extends Effectable.Class<number> {
  override = Effect.sync(() => Date.now())
}

const program = Effect.gen(function*() {
  const now = yield* new Now()
  return now
})
```

## Why This Changed

The v3 subtyping approach blurred the difference between "I have a handle" and
"I have an effect that reads or awaits that handle." v4 makes that boundary
explicit:

- `Ref` values are state cells; use `Ref.get`, `Ref.set`, or `Ref.update`.
- `Deferred` values are latches; use `Deferred.await`, `Deferred.succeed`, or
  `Deferred.fail`.
- `Fiber` values are running fibers; use `Fiber.join`, `Fiber.await`, or
  interruption APIs.
- Custom executable values should opt into the runtime using `Effectable`.

## Pitfalls

- Do not tell users to call `.asEffect()`; that belongs to an older beta.
- Do not yield raw `Ref`, `Deferred`, or `Fiber` values.
- Do not model `Option` / `Result` as Effect subtypes. Use `Option.match`,
  `Result.match`, or explicit conversion helpers at the boundary.
