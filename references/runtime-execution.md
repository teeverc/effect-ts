# Running Effects (Runtime Execution)

Use this guide when deciding how/where to run effects.

- The `run*` functions are the runtime entrypoints that execute effects.
- Keep `run*` calls at the program edge (CLI entrypoint, server bootstrap, tests).
- Use `Effect.runPromise` (or similar async variants) for async effects.
- Use `Effect.runSync` only for fully synchronous effects.
- Use `Effect.runFork` when you need a background fiber.

## Example
```ts
import * as Effect from "effect/Effect"
import * as Fiber from "effect/Fiber"

const asyncEffect = Effect.tryPromise({
  try: () => Promise.resolve("ok")
})

const syncEffect = Effect.sync(() => 123)

const fiber = Effect.runFork(asyncEffect)

Effect.runPromise(Fiber.join(fiber))
Effect.runSync(syncEffect)
```
