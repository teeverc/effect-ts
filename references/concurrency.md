# Concurrency (Fibers and Forking)

Use this guide when you need concurrent execution or background work.

- Effects run on fibers, which are lightweight virtual threads managed by the Effect runtime.
- `Effect.fork` starts an effect in a child fiber supervised by its parent.
- `Effect.forkDaemon` starts a global-scope fiber that is not supervised by a parent.
- `Effect.forkScoped` starts a child fiber tied to a local scope, independent of the parent.
- `Effect.forkIn` starts a child fiber in a specific scope for precise lifetime control.

## Example
```ts
import * as Effect from "effect/Effect"
import * as Fiber from "effect/Fiber"

const program = Effect.gen(function*() {
  const fiber = yield* Effect.fork(Effect.succeed(1))
  const result = yield* Fiber.join(fiber)
  return result
})

Effect.runPromise(program)
```
