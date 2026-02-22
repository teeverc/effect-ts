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

## Practical Example: Transformation and encoding

```ts
import { Effect, Schema } from "effect"

const User = Schema.Struct({
  id: Schema.Number,
  name: Schema.String,
  email: Schema.String
})

// Transform for API response (exclude sensitive fields)
const UserResponse = Schema.Struct({
  id: Schema.Number,
  name: Schema.String
})

const serializeUser = Schema.encodeEffect(
  Schema.Struct({
    id: Schema.Number,
    name: Schema.String
  })
)

const respond = (user: any) =>
  Effect.gen(function*() {
    const response = yield* serializeUser({
      id: user.id,
      name: user.name
    })
    return JSON.stringify(response)
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

// Union takes an array in v4
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

## Pitfalls

- Using sync decoders for async schemas.
- Skipping schema-based validation at boundaries.
- Relying on removed `validate*` APIs; use `decode*Effect` + `Schema.toType` instead.
- Forgetting that `Schema.Union` and `Schema.Tuple` take arrays in v4 (not varargs).
- Not providing `Schema.decodeUnknownEffect` at API boundaries; only use sync variants for trusted internal data.
