# Migration from Promise/async

Use this guide when converting Promise or async/await code to Effect.

- Promises are eager and single-shot; Effects are lazy and can be re-run.
- Effects model errors, interruption, and structured concurrency explicitly.
- Replace async/await flows with `Effect.gen` for similar readability.
- Replace `Promise.all` with `Effect.all` and choose concurrency options.
- Run effects at the boundary with `Effect.runPromise` (or other run* variants).

## Example

```ts
import * as Effect from "effect/Effect"

const program = Effect.async<number, never>((resume) => {
  resume(Effect.succeed(42))
})
```
