# Error Management (Expected vs Defects)

Use this guide when modeling failures in Effect.

- Treat *expected* errors as typed failures in the error channel.
- Treat *unexpected* errors as defects (unrecoverable bugs or unexpected exceptions).
- Keep domain errors explicit in effect types; handle them at the boundary where recovery is possible.
- When in doubt, start with a small, explicit error ADT and expand only as requirements demand.

## Example
```ts
import * as Data from "effect/Data"
import * as Effect from "effect/Effect"

class NotFound extends Data.TaggedError("NotFound")<{ readonly id: string }>() {}

const findUser = (id: string) =>
  id === "1"
    ? Effect.succeed({ id, name: "Ada" })
    : Effect.fail(new NotFound({ id }))

const program = findUser("2").pipe(
  Effect.catchAll((err) => Effect.succeed({ id: err.id, name: "guest" }))
)

Effect.runPromise(program)
```
