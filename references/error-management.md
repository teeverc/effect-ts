# Error Management (Expected vs Defects)

Use this guide when modeling failures in Effect.

- Treat *expected* errors as typed failures in the error channel.
- Treat *unexpected* errors as defects (unrecoverable bugs or unexpected exceptions).
- Keep domain errors explicit in effect types; handle them at the boundary where recovery is possible.
- When in doubt, start with a small, explicit error ADT and expand only as requirements demand.

## Example

```ts
import { Data, Effect } from "effect"

class NotFound extends Data.TaggedError("NotFound")<{}> {}

const program = Effect.fail(new NotFound()).pipe(
  Effect.catchTag("NotFound", () => Effect.succeed("guest"))
)
```
