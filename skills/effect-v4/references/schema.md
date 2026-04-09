# Effect Schema (v4) - Codec Pattern

Use this guide when you need validation, parsing, or encoding.

## Mental model

- **v4 change:** Schemas are now codecs (decode + encode) with transformation support.
- `decode*Effect` validates and transforms input to a typed value (replaces `decode*` from v3).
- `encode*Effect` converts typed values to an encoded representation (replaces `encode*` from v3).
- `validate*` APIs are removed; use `decode*Effect` + `Schema.toType` instead.
- Constructor-style schema APIs use `Schema.make(...)` again, not `Schema.makeUnsafe(...)`.
- Use `Schema.makeEffect(...)` when you want constructor-style parsing to fail in the error channel with `Schema.SchemaError`.

## Patterns

- Use `Schema.Struct` for objects.
- Use `Schema.NumberFromString` to parse string inputs.
- Use the newer `*FromString` and string-decoding schemas (`DateFromString`, `BigIntFromString`, `BigDecimalFromString`, `StringFromBase64`, `StringFromHex`, etc.) at text-heavy boundaries instead of manual preprocess transforms.
- Use `Schema.decodeUnknownEffect` for Effect-based decoding at boundaries.
- Use `Schema.make` on schema-backed classes or schemas when invalid input should throw synchronously.
- Use `Schema.makeEffect` when invalid input should be represented as `Effect` failure.
- Use `Schema.toType` / `Schema.toEncoded` when you need explicit type or encoded schemas.
- Use `Schema.asClass` when you want class ergonomics on top of an existing schema.
- Use `Schema.ArrayEnsure` when the input may be either a single value or an array but the output should always be an array.
- `Schema.Union` and `Schema.Tuple` take **arrays** in v4 (not varargs).

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

## Practical Example: API boundary validation

```ts
import { Effect, Schema } from "effect"

const UserRequest = Schema.Struct({
  id: Schema.NumberFromString,
  email: Schema.String,
  age: Schema.Optional(Schema.Number)
})

// Decode at the API boundary
const validateUserRequest = Schema.decodeUnknownEffect(UserRequest)

const apiHandler = (body: unknown) =>
  Effect.gen(function*() {
    // This will fail with detailed parse errors if body is invalid
    const user = yield* validateUserRequest(body)
    return `User ${user.email} (age ${user.age})`
  })
```

## Practical Example: Union and conditional parsing

```ts
import { Effect, Schema } from "effect"

const Circle = Schema.Struct({
  shape: Schema.Literal("circle"),
  radius: Schema.Number
})

const Square = Schema.Struct({
  shape: Schema.Literal("square"),
  side: Schema.Number
})

// Union takes an array in v4 (not varargs)
const Shape = Schema.Union([Circle, Square])

const decode = Schema.decodeUnknownEffect(Shape)

const area = (shapeData: unknown) =>
  Effect.gen(function*() {
    const shape = yield* decode(shapeData)
    const result = shape.shape === "circle"
      ? Math.PI * shape.radius ** 2
      : shape.side ** 2
    return result
  })
```

## Practical Example: `Schema.make` vs `Schema.makeEffect`

```ts
import { Effect, Schema } from "effect"

class Person extends Schema.Class<Person>("Person")({
  id: Schema.NumberFromString,
  name: Schema.String
}) {}

// Throws on invalid input
const person = Person.make({ id: "1", name: "Ada" })

// Fails in the Effect error channel with Schema.SchemaError
const safePerson = Person.makeEffect({ id: "1", name: "Ada" })

const program = Effect.gen(function*() {
  const value = yield* safePerson
  return value.name
})
```

## Recent Beta Notes

- `MakeOptions.disableValidation` was renamed to `disableChecks`.
- Constructor defaults still apply when `disableChecks: true` is used.
- `Schema.resolveAnnotationsKey` lets you inspect key-level annotations from a schema.
- `Schema.asClass` turns any schema into a class with static schema helpers.
- Schema-backed class APIs now enforce the `Self` generic more aggressively via clearer compile-time errors.

## Pitfalls

- Using sync decoders for async schemas.
- Skipping schema-based validation at boundaries.
- Relying on removed `validate*` APIs; use `decode*Effect` + `Schema.toType` instead.
- Forgetting that `Schema.Union` and `Schema.Tuple` take arrays in v4 (not varargs).
- Using stale beta docs that mention `Schema.makeUnsafe`; the current API is `Schema.make`.
- Using stale option names like `disableValidation`; the current constructor option is `disableChecks`.
- Not providing `Schema.decodeUnknownEffect` at API boundaries; only use sync variants for trusted internal data.

See `references/migration/` guides for detailed schema and codec changes.
