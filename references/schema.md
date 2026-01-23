# Effect Schema

Use this guide when you need validation, parsing, or encoding.

- `effect/Schema` provides schemas for validation and transformation.
- Use schemas to decode/encode data, assert invariants, or derive JSON Schema and pretty printers.
- Ensure TypeScript >= 5.4 and `strict` mode; consider enabling `exactOptionalPropertyTypes`.

## Example
```ts
import * as Effect from "effect/Effect"
import * as Schema from "effect/Schema"

const User = Schema.Struct({
  id: Schema.Number,
  name: Schema.String
})

const decodeUser = Schema.decodeUnknown(User)

const program = decodeUser({ id: 1, name: "Ada" })

Effect.runPromise(program)
```
