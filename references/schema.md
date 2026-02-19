# Effect Schema (v4)

Use this guide when you need validation, parsing, or encoding.

## Mental model

- Schemas are codecs (decode + encode) and may include transformations.
- `decode*Effect` validates and transforms input to a typed value.
- `encode*Effect` converts typed values to an encoded representation.

## Patterns

- Use `Schema.Struct` for objects.
- Use `Schema.NumberFromString` to parse string inputs.
- Use `Schema.decodeUnknownEffect` for Effect-based decoding at boundaries.
- Use `Schema.toType` / `Schema.toEncoded` when you need explicit type or encoded schemas.

## Walkthrough: decode and encode

```ts
import { Effect, Schema } from "effect"

const User = Schema.Struct({
  id: Schema.NumberFromString,
  name: Schema.String
})

const decode = Schema.decodeUnknownEffect(User)
const encode = Schema.encodeEffect(User)

const program = Effect.gen(function* () {
  const user = yield* decode({ id: "1", name: "Ada" })
  const encoded = yield* encode(user)
  return encoded
})
```

## Pitfalls

- Using sync decoders for async schemas.
- Skipping schema-based validation at boundaries.
- Relying on removed `validate*` APIs; use `decode*Effect` + `Schema.toType` instead.
