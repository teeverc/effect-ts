# Running Effects (Runtime Execution)

Use this guide when deciding how/where to run effects.

- The `run*` functions are the runtime entrypoints that execute effects.
- Keep `run*` calls at the program edge (CLI entrypoint, server bootstrap, tests).
- Use `Effect.runPromise` (or similar async variants) for async effects.
- Use `Effect.runSync` only for fully synchronous effects.
- Use `Effect.runFork` when you need a background fiber.

## Example

```ts
import { Effect, Runtime } from "effect"

const runtime = Runtime.defaultRuntime
const result = Runtime.runSync(runtime)(Effect.succeed(1))
```
