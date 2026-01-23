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

const fetchUser = async (id: string) => ({ id, name: `user-${id}` })

const fetchUserE = (id: string) =>
  Effect.tryPromise({
    try: () => fetchUser(id)
  })

const program = Effect.gen(function*() {
  const [a, b] = yield* Effect.all([fetchUserE("1"), fetchUserE("2")], { concurrency: 2 })
  return [a, b]
})

Effect.runPromise(program)
```
