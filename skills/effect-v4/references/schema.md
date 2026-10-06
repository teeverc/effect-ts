# Effect Schema (4.0) - Core Guide

Use this file for validation, parsing, construction, transformation and encoding with `Schema`.
For JSON Schema / representations, Standard Schema, `SchemaBinary`, Arbitrary, `TestSchema`,
`Model`/`VariantSchema`, the JIT/AOT compilers, and annotation internals, see `references/schema-advanced.md`.

Sources: `packages/effect/SCHEMA.md` upstream (https://github.com/Effect-TS/effect), `effect/Schema*.ts`.
v3 -> v4 renames: `references/migration/schema.md`, `references/migration/api-map/effect-Schema.md`.
Bundled example: `references/examples/01_effect/02_schema/10_schema-basics.ts`.

## Mental model and imports

- Every schema is a codec: `Schema.Codec<Type, Encoded, DecodingServices, EncodingServices>`.
  Decoding goes `Encoded -> Type`; encoding goes `Type -> Encoded`. `encode(s) === decode(flip(s))`.
- Read types with `typeof S.Type` / `typeof S.Encoded` (also `S["DecodingServices"]`).
- Core modules come from the root barrel: `import { Schema, SchemaAST, SchemaGetter, SchemaIssue, SchemaParser, SchemaTransformation } from "effect"`.
  `SchemaError` is a class exported from `Schema` (there is no `SchemaError` module).
- Use `Schema.Top` / `Schema.Codec` / `Schema.Constraint` as generic **constraints**
  (`<S extends Schema.Top>`); annotating a value as `Schema.Codec<...>` erases type info. Recursive schemas are the exception (see Recursion).
- Composite constructors take **arrays**: `Schema.Union([A, B])`, `Schema.Tuple([A, B])`, `Schema.Literals(["a", "b"])`, `Schema.TemplateLiteral([...])`.

## Built-in schemas (common)

| Kind | Schemas |
| --- | --- |
| Primitives | `String`, `Number`, `Boolean`, `BigInt`, `Symbol`, `Null`, `Undefined`, `Void`, `Unknown`, `Any`, `Never`, `ObjectKeyword` |
| Numbers | `Finite`, `Int`, `Natural` (non-negative safe int) |
| Strings | `NonEmptyString`, `Trimmed`, `Trim` (trims on decode), `Char` |
| Literals | `Literal("a")`, `Literals(["a","b"])` (`.literals`, `.pick([...])`, `.transform([...])`), `Enum`, `UniqueSymbol`, `TemplateLiteral`, `TemplateLiteralParser` |
| Parse from string | `NumberFromString` (may yield NaN), `FiniteFromString`, `BigIntFromString`, `BigDecimalFromString`, `DateFromString`, `DateTimeUtcFromString`, `DurationFromString`, `URLFromString`, `BooleanFromBit` |
| Other encodings | `DateFromMillis`, `DateTimeUtcFromMillis`, `StringFromBase64`, `StringFromBase64Url`, `StringFromHex`, `StringFromUriComponent`, `Uint8ArrayFromBase64`/`Base64Url`/`Hex`, `fromJsonString(S)` |
| Data types (no encoding) | `Date` (valid `Date` instances only), `DateTimeUtc`, `Duration`, `BigDecimal`, `Uint8Array`, `URL`, `Option(S)`, `Result(A, E)`, `Exit(...)`, `Cause(...)`, `Redacted(S)`, `Chunk`, `HashMap`, `HashSet`, `ReadonlyMap`, `ReadonlySet` |
| JSON | `Json`, `MutableJson`, `JsonObject` |
| Option bridges | `OptionFromNullOr`, `OptionFromUndefinedOr`, `OptionFromNullishOr`, `OptionFromOptionalKey`, `OptionFromOptional`, `OptionFromOptionalNullOr` |
| Misc | `RedactedFromValue(S)`, `ArrayEnsure(S)` (single or array -> array), `UniqueArray(S)`, `NonEmptyArray(S)`, `ErrorInstance()`, `Defect()` |

v4 note: `Schema.Date` is the old `DateFromSelf`. To parse ISO strings use `Schema.DateFromString`
(or `Schema.toCodecJson`, which encodes `Date` as an ISO string).

## Structs, optional keys, records, tuples

```ts
import { Schema, Struct } from "effect"

const User = Schema.Struct({
  id: Schema.FiniteFromString,
  name: Schema.NonEmptyString,
  email: Schema.optionalKey(Schema.String),   // { email?: string }             (exact optional)
  nick: Schema.optional(Schema.String),       // { nick?: string | undefined }
  bio: Schema.NullOr(Schema.String),          // also UndefinedOr / NullishOr
  tags: Schema.mutableKey(Schema.Array(Schema.String)),
  role: Schema.Literals(["admin", "member"])
})
type User = typeof User.Type

// Deriving structs: mapFields + Struct helpers
const Summary = User.mapFields(Struct.pick(["id", "name"]))
const Patch = User.mapFields(Struct.map(Schema.optionalKey))       // v3 partial({ exact: true })
const WithAge = User.pipe(Schema.fieldsAssign({ age: Schema.Int }))   // v3 extend
const Reused = Schema.Struct({ ...User.fields, extra: Schema.Boolean })

// Rename keys only on the encoded side
const Row = Schema.Struct({ userId: Schema.Number }).pipe(Schema.encodeKeys({ userId: "user_id" }))

const Scores = Schema.Record(Schema.String, Schema.Number)
const Pair = Schema.Tuple([Schema.String, Schema.Number])
const Head = Schema.TupleWithRest(Schema.Tuple([Schema.String]), [Schema.Number])
const Open = Schema.StructWithRest(Schema.Struct({ a: Schema.Number }), [Schema.Record(Schema.String, Schema.Number)])
```

- Excess keys are **stripped** by default. Pass `{ onExcessProperty: "error" }` to reject them; model extra keys with `Record`/`StructWithRest` to keep them. (`"preserve"` no longer exists.)
- `Schema.Record(Schema.Literals(["a","b"]), V)` yields a struct with required keys `a`, `b`.
- Arrays/tuples are readonly; `Schema.mutable(Schema.Array(X))` for mutable arrays.
- Other struct helpers: `Struct.omit`, `Struct.evolve`, `Struct.renameKeys`, `Struct.mapPick`, `Schema.requiredKey`; tuples use `.mapElements(Tuple.*)`, unions use `.mapMembers(Tuple.*)`.

## Unions and tagged unions

```ts
import { Schema } from "effect"

const IdOrName = Schema.Union([Schema.Int, Schema.NonEmptyString])       // first match wins
const Exactly = Schema.Union(
  [Schema.Struct({ a: Schema.String }), Schema.Struct({ b: Schema.Number })],
  { mode: "oneOf" }                                                    // exactly one member must match
)

const Shape = Schema.TaggedUnion({
  Circle: { radius: Schema.Finite },
  Square: { side: Schema.Finite }
})
const area = Shape.match({
  Circle: (c) => Math.PI * c.radius ** 2,
  Square: (s) => s.side ** 2
})
declare const value: typeof Shape.Type
Shape.guards.Circle(value)          // type guard per tag
Shape.isAnyOf(["Circle"])(value)
Shape.cases.Square                  // member schema
// also Shape.matchOrElse(value, { Circle: ... }, orElse)

// Existing union with a non-_tag discriminator:
const Event = Schema.Union([
  Schema.Struct({ type: Schema.tag("A"), a: Schema.String }),
  Schema.Struct({ type: Schema.tag("B"), b: Schema.Number })
]).pipe(Schema.toTaggedUnion("type"))
Schema.Literals(Event.discriminants)   // "A" | "B"
```

`Schema.TaggedStruct("A", { ... })` = `Struct({ _tag: Schema.tag("A"), ... })`; `make` fills `_tag` automatically.

## Classes and errors

```ts
import { Effect, Schema } from "effect"

class Person extends Schema.Class<Person>("app/Person")({
  id: Schema.Int,
  name: Schema.String
}) {
  get upper() { return this.name.toUpperCase() }
}
class Employee extends Person.extend<Employee>("app/Employee")({ salary: Schema.Finite }) {}

class Cat extends Schema.TaggedClass<Cat>()("Cat", { lives: Schema.Int }) {}

class NotFound extends Schema.TaggedError<NotFound>()("NotFound", { id: Schema.String }) {}
class Boom extends Schema.Error<Boom>("Boom")({ reason: Schema.String }) {}  // untagged yieldable error

const find = Effect.fn("find")(function*(id: string) {
  return yield* new NotFound({ id })   // schema errors are yieldable
})
const recovered = find("1").pipe(Effect.catchTag("NotFound", (e) => Effect.succeed(e.id)))
```

- `new Person({...})` validates and **throws** on invalid input. `Person.make`, `Person.makeOption`, `Person.makeEffect` are also available (see Constructors).
- Pass a `Struct(...).check(...)` instead of a field record to attach cross-field checks to a class.
- `TaggedClass`/`TaggedError`: tag doubles as identifier; pass `TaggedClass<Self>("custom-id")("Tag", {...})` to override.
- Brand a class: `Schema.Class<A, Brand.Brand<"A">>("A")({...})`. Keep base statics: `A.extend<B, typeof A>("B")({...})`.
- `Schema.Opaque<Self>()(Schema.Struct({...}))` gives a nominal type over a **plain** struct (no prototype, no methods, no `new`).
- Renamed in 4.0: `Schema.ErrorClass` -> `Schema.Error`, `Schema.TaggedErrorClass` -> `Schema.TaggedError`, error-instance schema -> `Schema.ErrorInstance`. `Schema.asClass` was removed (extend a schema directly: `class S extends Schema.String { static ... }`).
- `Data.TaggedError` still exists for errors that do not need a schema.

## Decoding and encoding APIs

`Schema.*` adapters fail with `Schema.SchemaError` (`.issue: SchemaIssue.Issue`, `.message`).
`SchemaParser.*` has the same names but fails/throws with the raw `SchemaIssue.Issue` (sync/promise variants throw an `Error` whose `cause` is the issue).

| Want | Decode (`unknown` input) | Decode (typed `Encoded` input) | Encode |
| --- | --- | --- | --- |
| Effect (async + services) | `decodeUnknownEffect` | `decodeEffect` | `encodeEffect` / `encodeUnknownEffect` |
| Exit | `decodeUnknownExit` | `decodeExit` | `encodeExit` / `encodeUnknownExit` |
| Result | `decodeUnknownResult` | `decodeResult` | `encodeResult` / `encodeUnknownResult` |
| Option | `decodeUnknownOption` | `decodeOption` | `encodeOption` / `encodeUnknownOption` |
| Promise | `decodeUnknownPromise` | `decodePromise` | `encodePromise` / `encodeUnknownPromise` |
| Sync (throws) | `decodeUnknownSync` | `decodeSync` | `encodeSync` / `encodeUnknownSync` |
| Guard / assert | `Schema.is(S)(u)` | `Schema.asserts(S, u)` (asserts on the `Type` side) | |

```ts
import { Effect, Schema } from "effect"

const decodeUser = Schema.decodeUnknownEffect(User)   // build once, reuse
const encodeUser = Schema.encodeEffect(User)

const handler = Effect.fn("handler")(function*(body: unknown) {
  const user = yield* decodeUser(body, { errors: "all", onExcessProperty: "error" })
  return yield* encodeUser(user)
})
```

- Parse options (`SchemaAST.ParseOptions`): `errors: "first" | "all"`, `onExcessProperty: "ignore" | "error"`,
  `concurrency` (struct/array/record children; union members stay sequential), `disableChecks`, `reportInput`.
  Options can be given when building the parser or per call (per-call wins).
- Sync/Option/Result/Promise adapters only work for schemas without services; async transformations fail them. Use `*Effect` at boundaries.
- Only causes made entirely of schema issues map to `None`/`false`/`SchemaError`; defects still throw/propagate.
- `validate*` APIs are gone: use `Schema.decodeUnknown*(Schema.toType(S))`. `Schema.toType(S)` / `Schema.toEncoded(S)` project one side; `Schema.flip(S)` swaps directions.
- `Schema.Decoder<T, RD>` / `Schema.Encoder<E, RE>` are narrow constraint types for decode-only / encode-only APIs.

## Constructors (`make`, `makeOption`, `makeEffect`)

Every schema (including unions) has constructor methods that apply constructor defaults and run **type-side** checks:

```ts
import { Effect, Schema } from "effect"

const Positive = Schema.Number.check(Schema.isGreaterThan(0))
Positive.make(1)                 // 1; throws Error("Schema validation failed") with issue in .cause
Positive.makeOption(-1)          // Option.none()
Positive.makeEffect(-1)          // Effect<number, SchemaIssue.Issue>
Positive.makeEffect(-1).pipe(Effect.mapError((issue) => new Schema.SchemaError(issue)))
Person.make({ id: 1, name: "Ada" }, { disableChecks: true })   // skip checks, defaults still apply
```

- Changed in 4.0: `makeEffect` fails with `SchemaIssue.Issue` (not `SchemaError`). Standalone forms: `SchemaParser.make(S)`, `SchemaParser.makeOption(S)`, `SchemaParser.makeEffect(S)`.
- `MakeOptions` = `{ disableChecks?, parseOptions? }` (`disableValidation` and `makeUnsafe` are old names).
- `Schema.make(ast)` is the low-level AST-to-schema wrapper, not a value constructor.
- Branded/refined schemas: standalone `make` accepts the unbranded input; inside a struct you must pass the branded value.

## Checks (filters), refinements, brands

```ts
import { Brand, Schema } from "effect"

const Password = Schema.String.check(
  Schema.isMinLength(8),
  Schema.isMaxLength(64),
  Schema.makeFilter((s) => /\d/.test(s) || "must contain a digit", { expected: "a string with a digit" })
)

const Signup = Schema.Struct({ password: Schema.String, confirm: Schema.String }).check(
  Schema.makeFilter((o) =>
    o.password === o.confirm ? undefined : { path: ["confirm"], issue: "passwords must match" }
  )
)

const NonEmpty = Schema.Array(Schema.String).pipe(
  Schema.refine((a): a is readonly [string, ...Array<string>] => a.length > 0, { expected: "a non-empty array" })
)

const Percent = Schema.Number.check(
  Schema.makeFilterGroup([Schema.isInt(), Schema.isBetween({ minimum: 0, maximum: 100 })], { title: "percent" })
)
const FastFail = Schema.String.check(Schema.isMinLength(3).abort(), Schema.isTrimmed())

// Brands: one concrete identifier per call, type-only (no runtime check, not in the AST)
const UserId = Schema.String.pipe(Schema.brand("UserId"))
type UserId = typeof UserId.Type        // string & Brand<"UserId">

// Reuse checks from Brand constructors; apply once per brand
type Int = number & Brand.Brand<"Int">
const Int = Brand.check<Int>(Schema.isInt())
type Pos = number & Brand.Brand<"Positive">
const Pos = Brand.check<Pos>(Schema.isGreaterThan(0))
const PositiveInt = Schema.Number.pipe(Schema.fromBrand("Int", Int), Schema.fromBrand("Positive", Pos))
```

- Built-in checks (all `Schema.is*`): strings `isMinLength`, `isMaxLength`, `isBetweenLength`, `isNonEmpty`, `isPattern`, `isTrimmed`, `isLowercased`, `isUppercased`, `isStartingWith`, `isEndingWith`, `isIncluding`, `isUUID`, `isULID`, `isGUID`, `isBase64`, `isBase64Url`, `isMinCodePoints`/`isMaxCodePoints`/`isBetweenCodePoints`; numbers `isGreaterThan`, `isGreaterThanOrEqualTo`, `isLessThan`, `isLessThanOrEqualTo`, `isBetween({ minimum, maximum })`, `isInt`, `isInt32`, `isFinite`, `isMultipleOf`; also `isBetweenDate`, `isBetweenBigInt`, `isBetweenBigDecimal`, `isUnique`, `isMinSize`/`isBetweenSize`, `isMinProperties`/`isBetweenProperties`. Factories for custom orders: `Schema.makeIsBetween({ order })`, `makeIsGreaterThan`, ...
- Changed in 4.0 stable: `isLengthBetween` -> `isBetweenLength` (subject last), `isStartsWith` -> `isStartingWith`, `isEndsWith` -> `isEndingWith`, `isIncludes` -> `isIncluding`. No `positive`/`nonNegative` filters: use `isGreaterThan(0)` / `isGreaterThanOrEqualTo(0)`.
- Filter predicate return shapes: `true | undefined` (ok), `false`, `string` (message), `SchemaIssue.Issue`, `{ path, issue }`, or an array of those.
- Checks do not change the schema type: `.fields`, `.make` etc. remain after `.check(...)`.
- `.check` filters are synchronous. For effectful/service-dependent validation use `SchemaGetter.checkEffect` in a transformation (see below).
- Structural checks (array length, object size) run only after children parse successfully.
- Changed in 4.0.0: `Schema.brand` takes exactly one concrete identifier (`Schema.brand("A")`; no `brand<"A">()`, no unions) and is type-only, so `SchemaRepresentation` drops it. `fromBrand(identifier, ctor)` requires the constructor's sole brand key (pass enum members for enum keys).

## Transformations

```ts
import { Effect, Schema, SchemaGetter, SchemaIssue, SchemaTransformation } from "effect"

// Same type on both sides: Schema.decode
const Lower = Schema.String.pipe(Schema.decode(SchemaTransformation.composeTransformation(
  SchemaTransformation.trim(),
  SchemaTransformation.toLowerCase()
)))

// Different target: source.pipe(Schema.decodeTo(target, transformation))
const Km = Schema.Finite.pipe(Schema.decode(SchemaTransformation.transform({
  decode: (meters) => meters / 1000,
  encode: (km) => km * 1000
})))

const UrlFromString = Schema.String.pipe(
  Schema.decodeTo(
    Schema.instanceOf(URL),
    SchemaTransformation.transformEffect({
      decode: (s, options) =>
        Effect.try({
          try: () => new URL(s),
          catch: () => new SchemaIssue.InvalidValue({ message: "Invalid URL" }, s, options)
        }),
      encode: (url) => Effect.succeed(url.href)
    })
  )
)

// Getter-level form: { decode: Getter, encode: Getter }
const Len = Schema.String.pipe(Schema.decodeTo(Schema.Number, {
  decode: SchemaGetter.transform((s: string) => s.length),
  encode: SchemaGetter.transform((n: number) => "x".repeat(n))
}))

// Schema composition (no transformation): chain codecs
const Composed = Schema.String.pipe(Schema.decodeTo(Schema.FiniteFromString))   // types must line up
```

- `Schema.encodeTo(target, t)` / `Schema.encode(t)` are the inverse-direction forms.
- `SchemaTransformation`: `transform`, `transformEffect`, `transformOptional`, `makeTransformation`, `composeTransformation`, `passthrough()` (with `{ strict: false }`), `passthroughSubtype()`, `passthroughSupertype()`, plus built-ins (`trim`, `snakeToCamel`, `numberFromString`, `dateFromString`, `fromJsonString(options)`, ...).
- `SchemaGetter`: `transform`, `transformEffect`, `transformOptional`, `transformOptionalEffect`, `passthrough`, `checkEffect`, `omit`, `withDefault`, `succeed`, `fail`, `forbidden`, `forbiddenEncoding` (decode-only transformations), plus string helpers. Compose with standalone `SchemaGetter.map` / `SchemaGetter.compose`.
- Changed in 4.0 stable: `transformOrFail` -> `transformEffect` (both modules); `SchemaTransformation.make` -> `makeTransformation`; `t1.compose(t2)` -> `SchemaTransformation.composeTransformation(t1, t2)`; `new SchemaGetter.Getter`, `onSome`, `onNone` removed (use `transformEffect` / `transformOptionalEffect`); getters expose only `pipe`.
- Failures inside transformations must be `SchemaIssue.Issue` values (e.g. `new SchemaIssue.InvalidValue(annotations, input, options)`; passing `options` honors `reportInput`).
- Fallbacks: `S.pipe(Schema.catchDecoding(() => Effect.succeedSome(fallback)))`; `Effect.succeedNone` omits an optional key. `catchDecodingWithContext` for service-based fallbacks.

## Defaults

| API | Applies when | Default given as |
| --- | --- | --- |
| `withDecodingDefaultKey(eff)` | key absent while decoding | `Encoded` value |
| `withDecodingDefault(eff)` | key absent or `undefined` | `Encoded` value |
| `withDecodingDefaultTypeKey(eff)` | key absent | `Type` value |
| `withDecodingDefaultType(eff)` | key absent or `undefined` | `Type` value |
| `withConstructorDefault(eff)` | `make`/`new` without the key (or `undefined`) | `Type` value |
| `Schema.tagDefaultOmit("X")` | decode fills tag, encode omits it | literal |

```ts
import { Effect, Schema } from "effect"

const Config = Schema.Struct({
  port: Schema.FiniteFromString.pipe(Schema.withDecodingDefault(Effect.succeed("8080"))),
  retries: Schema.Int.pipe(Schema.withDecodingDefaultType(Effect.succeed(3))),
  createdAt: Schema.Date.pipe(Schema.withConstructorDefault(Effect.sync(() => new Date())))
})
```

- Default effects run every time a default is needed. Decoding defaults take `Effect<_, Schema.SchemaError, R>` and may require services (they appear in `DecodingServices`); the second arg `{ encodingStrategy: "omit" }` drops the key on encode. Constructor defaults take `Effect<_, SchemaIssue.Issue>` with no required services (optional services via `Effect.serviceOption` are fine) and still apply with `disableChecks: true`.
- Use `Schema.OptionFromOptionalKey(S)` / `OptionFromNullOr(S)` etc. when the domain type should be `Option`.

## Serialization and canonical codecs

```ts
import { Schema, SchemaTransformation } from "effect"

const Event = Schema.Struct({ at: Schema.Date, n: Schema.BigInt, tag: Schema.Option(Schema.String) })

const EventJson = Schema.toCodecJson(Event)                 // Date -> ISO string, bigint -> string, Option -> tagged JSON
const json = Schema.encodeSync(EventJson)(value)            // JSON-safe value
const fromText = Schema.fromJsonString(EventJson)           // string <-> JSON.parse + decode
const st = Schema.toCodecStringTree(Schema.Struct({ page: Schema.Int }))  // every leaf a string
const qs = Schema.fromURLSearchParams(st)                   // also Schema.fromFormData(st); bracket notation for nesting
const iso = Schema.toCodecIso(Person)                      // class <-> plain fields

// Custom types: declare a JSON representation once (also drives JSON Schema output)
class Point { constructor(readonly x: number, readonly y: number) {} }
const PointSchema = Schema.instanceOf(Point, {
  expected: "Point",
  toCodecJson: () =>
    Schema.link<Point>()(
      Schema.Tuple([Schema.Finite, Schema.Finite]),
      SchemaTransformation.transform({ decode: ([x, y]) => new Point(x, y), encode: (p) => [p.x, p.y] as const })
    )
})
```

- Explicit encodings win over canonical defaults (`toCodecJson` keeps your `encodeTo`).
- Without a `toCodecJson` annotation, declarations encode as `Schema.Json` (encoding a non-JSON instance fails with "Expected JSON value").
- `Schema.declare(guard, { expected })` / `Schema.declareConstructor<T, E>()(typeParams, run, annotations)` define custom (parametric) types.
- `Schema.toEncoderXml(S)` returns `(t) => Effect<string, SchemaIssue.Issue>`; `Schema.toEquivalence(S)`, `Schema.toIso(S)`, `Schema.toDifferJsonPatch(S)` are other derivations.
- `Schema.toCodecStringTree` no longer accepts `keepDeclarations`. `Schema.UnknownFromJsonString` is internal: use `Schema.fromJsonString(Schema.Unknown)`.

## Errors and formatting

```ts
import { Effect, Schema, SchemaIssue } from "effect"

class InvalidInput extends Schema.TaggedError<InvalidInput>()("InvalidInput", { message: Schema.String }) {}

const program = Schema.decodeUnknownEffect(User)(input, { errors: "all" }).pipe(
  Effect.catchTag("SchemaError", (error) => {
    // error.message is the default formatted text
    const { issues } = SchemaIssue.makeFormatterStandardSchemaV1()(error.issue)  // [{ path?, message }]
    const text = SchemaIssue.makeFormatterDefault()(error.issue)
    return Effect.fail(new InvalidInput({ message: text }))
  })
)
```

- Issue tags: `InvalidType`, `InvalidValue`, `MissingKey`, `UnexpectedKey`, `Forbidden`, `OneOf`, `AnyOf`, `Filter`, `Encoding`, `Pointer`, `Composite`. Formatters accept `leafHook` / `checkHook` for i18n.
- Custom messages: `.annotate({ message })` on a schema, `{ message }` or `{ expected }` on a filter, `.annotateKey({ messageMissingKey })` on a field, `.annotate({ messageUnexpectedKey })` on a struct. `identifier` names type-level failures only ("Expected Username"), not failed filters.
- Inputs are not retained in issues by default; `{ reportInput: true }` adds `issue.input` (check with `SchemaIssue.hasInput`). Beware leaking secrets/PII.
- `Issue#toString` is not a formatter; use `SchemaIssue.makeFormatterDefault()`. Sync/promise `SchemaParser` adapters throw `"Schema validation failed"` with the issue in `cause`.
- Ship failures over the wire with `Schema.toCodecJson(Schema.StandardSchemaV1FailureResult)`.

## Effectful decoding with services

```ts
import { Context, Effect, Schema, SchemaGetter } from "effect"

class Users extends Context.Service<Users, { readonly exists: (id: string) => Effect.Effect<boolean> }>()("app/Users") {}

const ExistingUserId = Schema.String.pipe(Schema.decode({
  decode: SchemaGetter.checkEffect((id) =>
    Effect.gen(function*() {
      const users = yield* Users
      return (yield* users.exists(id)) || "unknown user"
    })
  ),
  encode: SchemaGetter.passthrough()
}))
// Effect<string, Schema.SchemaError, Users>
const decoded = Schema.decodeUnknownEffect(ExistingUserId)("u1")
// Or bake the service into the schema:
const Provided = ExistingUserId.pipe(
  Schema.middlewareDecoding(Effect.provideService(Users, Users.of({ exists: () => Effect.succeed(true) })))
)
```

Decoding and encoding services are tracked separately (`RD`, `RE`); encoding this schema needs nothing.

## Void vs Undefined

- `Schema.Void` models an ignored `void` return value: any present value decodes to `undefined`. Use it for RPC/endpoint results whose value is discarded.
- `Schema.Undefined` accepts only the exact `undefined` value. Use it when `undefined` must be validated.

## Recursion

```ts
import { Schema } from "effect"

interface Category { readonly name: string; readonly children: ReadonlyArray<Category> }
const Category: Schema.Codec<Category> = Schema.Struct({
  name: Schema.String,
  children: Schema.Array(Schema.suspend((): Schema.Codec<Category> => Category))
})
```

Use `Schema.Codec<Type, Encoded>` when the sides differ; classes use `Schema.suspend((): Schema.Codec<Self> => Self)` inside their fields.

## Pitfalls

- Using `Schema.makeUnsafe`, `SchemaParser.makeUnsafe`, `Schema.asClass`, `Schema.ErrorClass`, `transformOrFail`, `disableValidation`, `Schema.toArbitrary`, or `effect/unstable/schema/*` paths: all stale. Schema modules import from `effect`; `Model`/`VariantSchema`/compilers from `effect/schema`.
- Expecting `makeEffect` to fail with `SchemaError` (it fails with `SchemaIssue.Issue`).
- `Schema.brand<"A">()` or multi-key brands (use `Schema.brand("A")`, once per brand).
- Using `Schema.Date` to parse strings (use `DateFromString`) or `NumberFromString` when NaN must be rejected (use `FiniteFromString`).
- Varargs `Union(A, B)` / `Literal("a", "b")` (use arrays / `Literals`).
- Sync decoders with async transformations or service requirements.
- Relying on decoded key order (`propertyOrder` parse option was removed).
- Passing transformed parts (e.g. `FiniteFromString`) to `TemplateLiteral` (throws); use `TemplateLiteralParser`.
