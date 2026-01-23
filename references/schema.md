# Effect Schema

Use this guide when you need validation, parsing, or encoding.

- `effect/Schema` provides schemas for validation and transformation.
- Use schemas to decode/encode data, assert invariants, or derive JSON Schema and pretty printers.
- Ensure TypeScript >= 5.4 and `strict` mode; consider enabling `exactOptionalPropertyTypes`.

## Example

```ts
import { Schema } from "effect"

const User = Schema.Struct({
  id: Schema.NumberFromString
})

const decode = Schema.decode(User)
const program = decode({ id: "1" })
```
