# Effect Schema (4.0) - Advanced Topics

Companion to `references/schema.md`. Covers JSON Schema and `SchemaRepresentation`, Standard Schema,
`SchemaBinary`, native `Arbitrary`, `TestSchema`, `Model`/`VariantSchema`, the JIT/AOT compilers,
effectful decoding with services, and annotations.

Stability (verify per module): `Schema`, `SchemaAST`, `SchemaIssue`, `SchemaParser`, `SchemaGetter`,
`SchemaTransformation`, `SchemaRepresentation`, `JsonSchema`, `StandardSchema` have no stability tag (semver).
`Arbitrary`, `effect/testing` `TestSchema`, `effect/encoding` `SchemaBinary`, and everything under `effect/schema`
(`Model`, `VariantSchema`, `SchemaCompiler`, `SchemaJITCompiler`, `SchemaAOTCompiler`) are `@stability unstable`
(breaking changes allowed in minor releases).

| Module | Import |
| --- | --- |
| `SchemaRepresentation`, `JsonSchema`, `Arbitrary`, `StandardSchema` | `import { Arbitrary, JsonSchema, SchemaRepresentation } from "effect"` |
| `TestSchema` | `import { TestSchema } from "effect/testing"` |
| `SchemaBinary` | `import { SchemaBinary } from "effect/encoding"` |
| `Model`, `VariantSchema`, `SchemaJITCompiler`, `SchemaAOTCompiler`, `SchemaCompiler` | `import { Model } from "effect/schema"` (or `effect/schema/Model`) |

## JSON Schema export

```ts
import { JsonSchema, Schema } from "effect"

const User = Schema.Struct({
  id: Schema.Int,
  name: Schema.NonEmptyString.annotate({ description: "display name" })
}).annotate({ identifier: "User" })        // identifier => becomes a $defs reference

const doc = Schema.toJsonSchemaDocument(User)
// { dialect: "draft-2020-12", schema: { $ref: "#/$defs/User" }, definitions: { User: {...} } }
const closed = Schema.toJsonSchemaDocument(User, { onExcessProperty: "error" })  // additionalProperties: false
const draft07 = JsonSchema.toDocumentDraft07(doc)    // also toDocumentDraft04, toMultiDocumentOpenApi3_1
```

- `toJsonSchemaDocument` derives `Schema.toCodecJson(schema)` first, then describes the **encoded** JSON shape (e.g. `Date` -> `string`, custom types via their `toCodecJson` annotation). `fromJsonString(S)` emits `contentMediaType` + `contentSchema`.
- Default `onExcessProperty: "ignore"` emits `additionalProperties: true` (matching the decoder). Use `"error"` in both generation and decoding for closed objects. (The old `additionalProperties` option was replaced by `onExcessProperty`.)
- `title`, `description`, `default`, `examples`, `readOnly`, `writeOnly`, `format` annotations are emitted; checks become `allOf` fragments. `.annotate` on a transformation annotates the decoded side; use `Schema.annotateEncoded({...})` to annotate what JSON Schema sees.
- Generated JSON Schema is a preliminary check; the Effect decoder is the final authority (code-point vs UTF-16 lengths, RegExp flags, approximate checks). Unions with approximate branches export `anyOf` instead of `oneOf`.
- Options also accept `referencePolicy: ({ ast, occurrences, identifier }) => string | undefined` to control which nodes become `$defs`.
- Custom check export: a filter annotation `toJsonSchema` returns a fragment (exact) or `[fragment, true]` (safe approximation; `[{}, true]` to omit).
- Other dialects in: `JsonSchema.fromSchemaDraft07(js)`, `fromSchemaDraft2020_12(js)`, `fromSchemaOpenApi3_1(js)`, `fromSchemaOpenApi3_0(js)` all return a `Document<"draft-2020-12">`.
- `Schema.toStandardJSONSchemaV1(S)` exposes the experimental Standard JSON Schema interface.

## JSON Schema import

```ts
import { Schema, SchemaRepresentation } from "effect"

const Imported = SchemaRepresentation.fromJsonSchemaDocument(doc)             // Schema.Top
const decode = Schema.decodeUnknownEffect(Imported)
const Trusted = SchemaRepresentation.fromJsonSchemaDocument(doc, { patterns: "apply" })
const [A, B] = SchemaRepresentation.fromJsonSchemaMultiDocument(multiDoc)
```

- Input must be a valid Draft 2020-12 document (convert others with `JsonSchema.fromSchema*`). Only local `#/$defs/<name>` refs are supported.
- `patterns`: `"error"` (default; reject regex constraints, ReDoS safety), `"ignore"`, or `"apply"` (trusted docs only). `onEnter` can normalize each node.
- Best-effort, not lossless: unsupported keywords (`$dynamicRef`, `contains`, `dependentSchemas`, active `if/then/else`, `unevaluated*`, object/array `const`/`enum`) throw with a path. Open objects import with a `Schema.Json` index signature. `minLength`/`maxLength` map to code-point checks.
- The result type is `Schema.Top` (unknown services), so use `decodeUnknownEffect`, `Schema.is`, or narrow it yourself.

## SchemaRepresentation (persist / rebuild / codegen)

A `Document` is `{ representation, references }`; a `MultiDocument` has several roots sharing `references`.
Representations project the **encoded** side by default (`Schema.toType(S)` first for the type side), never serialize
transformation functions, and drop type-only operations (`Schema.brand`, refinement narrowing).

```ts
import { Schema, SchemaRepresentation } from "effect"

const live = Schema.toRepresentation(Schema.String.check(Schema.isMinLength(3)))
const json = SchemaRepresentation.toJson(live)                 // Schema.Json; store or send it
const persisted = SchemaRepresentation.fromJson(json)          // validated Document (no callbacks)
const rebuilt = SchemaRepresentation.fromRepresentation(persisted, {
  revivers: [SchemaRepresentation.isMinLengthReviver]          // required, even if []
})
Schema.is(rebuilt)("abc")  // true

// Codegen: runtime + TypeScript source fragments
const multi = SchemaRepresentation.toRepresentations([User.ast, Other.ast])
const { codes, references, artifacts } = SchemaRepresentation.toCodeDocument(multi)  // codes[i].runtime / .Type
```

- Built-in revivers are individual exports (`OptionReviver`, `DateReviver`, `isMinLengthReviver`, `isPatternReviver`, ...). Missing/duplicate ids or bad payloads are errors.
- Custom persistable checks/declarations: put `representation: { id, payload }` in the filter/declaration annotations and provide a reviver from `makeReviverFilter(id, payloadSchema, ({ payload, annotations }) => filter)`, `makeReviverDeclaration`, or `makeReviverFilterGroup`.
- `SchemaRepresentation.toJsonSchemaDocument(liveDoc)` / `toJsonSchemaMultiDocument` compile live documents. Compile before `toJson`, since callbacks (`toJsonSchema`, `toCode`) do not persist.
- 4.0 changes: `toSchema` -> `fromRepresentation` (revivers now explicit, `toSchemaDefaultReviver` removed); `SchemaMultiDocument`/`fromSchemaMultiDocument` removed; reviver constructors are `makeReviver*`; the persisted format is incompatible with pre-release documents.

## Standard Schema

```ts
import { Schema } from "effect"

const standard = Schema.toStandardSchemaV1(User, { parseOptions: { errors: "all" } })  // also leafHook / checkHook
const result = standard["~standard"].validate(input)  // { value } | { issues: [{ message, path }] } (Promise if async)
```

- Returns `StandardSchemaV1<Encoded, Type> & S`, so it remains usable as a normal schema (TanStack Form, tRPC, Elysia, etc.).
- Types are vendored in the `StandardSchema` module (`StandardSchemaV1`, `StandardJSONSchemaV1`, `StandardTypedV1`); `effect` no longer depends on `@standard-schema/spec`.
- `SchemaIssue.makeFormatterStandardSchemaV1(hooks?)` converts an `Issue` into a `FailureResult`; `Schema.StandardSchemaV1FailureResult` serializes it.

## SchemaBinary (compact binary codec)

```ts
import { Schema } from "effect"
import { SchemaBinary } from "effect/encoding"

const Person = Schema.Struct({ id: Schema.String.pipe(SchemaBinary.fieldId(1)), age: Schema.Int })
const codec = SchemaBinary.toCodec(Person)            // Codec<Person, Uint8Array<ArrayBuffer>>
const bytes = Schema.encodeUnknownSync(codec)({ id: "a", age: 36 })
const back = Schema.decodeUnknownSync(codec)(bytes)
const small = SchemaBinary.toCodec(Person, { fingerprint: true })   // positional; peers need identical schemas

const parser = SchemaBinary.parser(Person, { maxFrameSize: 1 << 20 })
parser.feedSync(chunk)    // completed values; parser.feed(chunk) is the Effect form; end / endSync()
```

- Built from the encoded side; default mode tolerates compatible schema evolution. `fieldId(n)` pins a wire id across renames.
- Encoded results are arena-backed views; call `bytes.slice()` for independent ownership.
- Streaming: `SchemaBinary.encode(S)` / `decode(S)` return Channel factories; `duplex` and `encoder` cover connection-scoped dictionaries. Failures are `Schema.SchemaError`.
- Replaces MessagePack (removed): event-log, RPC and cluster serialization use SchemaBinary.

## Arbitrary (native property testing)

`Arbitrary` is a top-level module (`import { Arbitrary } from "effect"`); fast-check is no longer used or re-exported.
`Schema.toArbitrary` and `effect/testing/FastCheck` were removed. See `references/migration/schema.md` (native Arbitrary section) and `references/migration/api-map/effect-Arbitrary.md`.

```ts
import { Arbitrary, Effect, Schema } from "effect"

const arb = Arbitrary.schema(User)                                   // Arbitrary<User>
const samples = Arbitrary.sampleEffect(arb, { count: 5, seed: 1 })   // Effect<ReadonlyArray<User>, SampleError>

const check = Arbitrary.checkEffect(arb, (u) => u.name.length > 0, { runs: 100, seed: 42 })
const program = Effect.map(check, (result) => Arbitrary.formatCheckFailure(result))  // undefined when Passed
// result._tag: "Passed" | "Falsified" (initialInput, shrunkInput, replay) | "Exhausted" | "ReplayMismatch"

const commands = Arbitrary.array(Arbitrary.schema(Schema.Literals(["Add", "Reset"])), { maxLength: 50 })
const pair = Arbitrary.all([arb, Arbitrary.Constant(1)])     // tuples or records
const ids = arb.pipe(Arbitrary.map((u) => u.id))             // also filter, filterMap, flatMap
Arbitrary.configureGlobal({ check: { runs: 50 } })           // {} resets
```

- Properties may return `boolean` or `Effect<boolean, E, R>`; `false` and failures shrink; defects/interruption propagate.
- Replay a failure with `{ replay: result.replay }` (tokens are not stable across releases).
- Generation honors schema checks (`arbitraryConstraint` metadata on built-in filters). For custom declarations, provide a `toCodecArbitrary` annotation (experimental) or a `toCodecJson` link; custom shrinking via `Arbitrary.schema(S, { shrink })`.
- `@effect/vitest`: `it.prop("name", [Schema.Int, arb], ([a, b]) => ...)` and `it.effect.prop(...)`; pass `{ arbitrary: CheckOptions }` as the timeout/options argument.

## TestSchema

```ts
import { Effect, Schema } from "effect"
import { TestSchema } from "effect/testing"

const asserts = new TestSchema.Asserts(Schema.FiniteFromString)
await asserts.decoding().succeed("1", 1)
await asserts.decoding().fail("a", "<expected formatted message>")
await asserts.encoding().succeed(1, "1")
await asserts.make().succeed(1)
await asserts.verifyRoundTrip({ seed: 1, runs: 20 })      // encode -> decode == original, shrinks failures
asserts.arbitrary().verifyGeneration()                    // generated values satisfy the schema

// Effect forms use the calling fiber's services / TestClock
const test = Effect.gen(function*() {
  yield* asserts.decoding().succeedEffect("42", 42)
  yield* asserts.verifyRoundTripEffect({ seed: 1, runs: 20 })
})
```

- Changed in 4.0.0: `verifyLosslessTransformation` -> `verifyRoundTrip` (plus `verifyRoundTripEffect`, `succeedEffect`, `failEffect`).
- `decoding({ parseOptions })` / `encoding(...)` accept options; `.provide(Service, impl)` injects services via middleware. `TestSchema.Asserts.ast.fields.equals(a, b)` compares field ASTs.
- `fail` messages are `SchemaIssue.makeFormatterDefault()` output.

## Model and VariantSchema (`effect/schema`)

`Model.Class` declares fields once and derives six variants: `select` (the class itself), `insert`, `update`,
`json`, `jsonCreate`, `jsonUpdate`. Used by `effect/sql` `SqlModel.makeRepository` / `makeResolvers`.

```ts
import { Schema } from "effect"
import { Model } from "effect/schema"

export const GroupId = Schema.Number.pipe(Schema.brand("GroupId"))

export class Group extends Model.Class<Group>("Group")({
  id: Model.GeneratedByDb(GroupId),           // select + json only
  name: Schema.String,
  passwordHash: Model.Sensitive(Schema.String),  // db variants only, never JSON
  note: Model.FieldOption(Schema.String),     // Option in every variant (nullable / optional key)
  createdAt: Model.DateTimeInsertFromDate,    // set on insert
  updatedAt: Model.DateTimeUpdateFromDate     // set on insert + update
}) {}

Group.insert      // schema for INSERT payloads
Group.jsonCreate  // schema for API create bodies
class GroupJson extends Schema.Class<GroupJson>("GroupJson")(Group.json) {}
```

- Field helpers: `Model.Field({ select, insert, ... })`, `FieldOnly`, `FieldExcept`, `fieldEvolve`, `GeneratedByApp`, `Sensitive`, `optionalOption`, `FieldOption`, `BooleanSqlite`, `JsonFromString(S)`, `Date`, `DateTimeInsert*` / `DateTimeUpdate*` (`FromDate`, `FromNumber`), `DateTimeWithNow`, `UuidV4Insert(brandedStringSchema)`, `UuidV7Insert(brandedStringSchema)` (e.g. `Schema.String.pipe(Schema.brand("GroupId"))`), `Model.Override(value)`.
- `Model.Struct`, `Model.Union`, `Model.extract(model, "json")`, `Model.fields(model)` for non-class usage.
- `VariantSchema.make({ variants: ["db", "api"], defaultVariant: "db" })` returns `{ Class, Field, FieldOnly, FieldExcept, Struct, Union, extract, fieldEvolve }` for custom variant sets:

```ts
import { Schema } from "effect"
import { VariantSchema } from "effect/schema"

const { Class, Field } = VariantSchema.make({ variants: ["db", "api"], defaultVariant: "db" })
class Account extends Class<Account>("Account")({
  id: Schema.String,
  hash: Field({ db: Schema.String })   // absent from the api variant
}) {}
```

## Experimental JIT / AOT compilers (`effect/schema`, unstable)

Speed up parsing without changing results. Both fill one registry keyed by exact AST; anything unsupported falls back to the interpreter.

```ts
// JIT for the whole app (side-effect import at startup)
import "effect/schema/SchemaJITCompiler/enable"

// or per schema
import { SchemaJITCompiler } from "effect/schema"
SchemaJITCompiler.enable(User.ast)
```

```ts
// AOT: generate a module (no `new Function` at runtime, CSP-safe)
import { SchemaAST } from "effect"
import { SchemaAOTCompiler } from "effect/schema"
import * as Build from "effect/schema/SchemaAOTCompiler/Build"

const source: string = SchemaAOTCompiler.compile([
  { ast: User.ast, operations: ["decode"] },
  { ast: SchemaAST.toType(User.ast), operations: ["is", "make"] }   // guards / construction use the type side
])
// the generated module exports install(asts); call it with the same ASTs in the same order

// Higher-level build step (needs FileSystem + Path, e.g. NodeServices.layer)
Build.build({
  modules: { "./schemas/User.js": () => import("./schemas/User.js") },
  baseUrl: import.meta.url,
  outFile: "./generated/schema-aot.js",
  operations: ["decode", "encode"]   // default ["decode"]; also "is", "make"
})
```

- Importing `SchemaJITCompiler` or the `effect/schema` barrel alone does not enable anything.
- Encoding AOT targets `SchemaAST.flip(ast)` with `decode`. Regenerate AOT output when schemas or the Effect version change. Checks/getters on replayed paths must be deterministic and side-effect free.
- `SchemaCompiler.set(ast, decoder)` installs a hand-written decoder (trusted).

## Effectful decoding with services

Schemas carry separate `DecodingServices` (`RD`) and `EncodingServices` (`RE`). Sources of requirements:
`SchemaGetter.transformEffect`/`checkEffect`/`transformOptionalEffect` getters, `SchemaTransformation.transformEffect`,
`Schema.withDecodingDefault*` effects, `Schema.catchDecodingWithContext`.

```ts
import { Context, Effect, Schema, SchemaGetter, SchemaIssue } from "effect"

class Users extends Context.Service<Users, {
  readonly find: (id: string) => Effect.Effect<{ readonly id: string; readonly name: string } | undefined>
}>()("app/Users") {}

const UserFromId = Schema.String.pipe(Schema.decodeTo(
  Schema.Struct({ id: Schema.String, name: Schema.String }),
  {
    decode: SchemaGetter.transformEffect((id: string, options) =>
      Effect.gen(function*() {
        const user = yield* (yield* Users).find(id)
        return user ?? (yield* Effect.fail(new SchemaIssue.InvalidValue({ message: "unknown user" }, id, options)))
      })
    ),
    encode: SchemaGetter.transform((user: { readonly id: string }) => user.id)
  }
))

// Effect<{...}, Schema.SchemaError, Users>; encoding requires nothing
const decoded = Schema.decodeUnknownEffect(UserFromId)("u1")

// Provide inside the schema instead:
const Standalone = UserFromId.pipe(Schema.middlewareDecoding(Effect.provideService(Users, Users.of({
  find: (id) => Effect.succeed({ id, name: "Ada" })
}))))
```

- Service-requiring schemas cannot use `*Sync`/`*Option`/`*Result`/`*Promise` adapters (type error).
- `Schema.middlewareDecoding(f)` / `middlewareEncoding(f)` wrap the parse effect (provide services, add spans, recover).
- Product parsing can be concurrent with `{ concurrency: n | "unbounded" }`; union members stay sequential.

## Annotations

```ts
import { Schema } from "effect"

const Username = Schema.NonEmptyString.annotate({
  identifier: "Username",           // name for $defs, formatter type-level failures
  title: "Username",
  description: "Login name",
  examples: ["ada"],
  default: "anonymous",
  message: "Enter a username"        // replaces the whole message for this node
})
const Form = Schema.Struct({
  username: Username.annotateKey({ description: "field docs", messageMissingKey: "Username is required" })
}).annotate({ messageUnexpectedKey: "Unknown field" })

Schema.resolveAnnotations(Username)?.title       // reads the last check's annotations first, else the schema's
Schema.resolveAnnotationsKey(Form.fields.username)

// Typed custom annotations via module augmentation
declare module "effect/Schema" {
  namespace Annotations {
    interface Augment {
      readonly version?: readonly [major: number, minor: number, patch: number] | undefined
    }
  }
}
```

- Filter annotations (`Schema.makeFilter(p, { ... })`, `Schema.isX({ ... })`): `expected`, `message`, `title`, `description`, `identifier`, plus `representation`, `toJsonSchema`, `toCode`, `arbitraryConstraint`.
- Declaration annotations (`Schema.declare`, `Schema.instanceOf`): `expected`, `toCodecJson`, `toCodecStringTree`, `toCodecIso`, `toCodec`, `toCodecArbitrary`, `toEquivalence`, `toFormatter`, `representation`, `toCode`.
- Override derived behavior: `Schema.overrideToEquivalence(() => eq)`, `Schema.overrideToFormatter(...)`, `Schema.overrideToCodecIso(...)`.
- Classes take annotations as the second argument: `Schema.Class<A>("A")({ ... }, { title: "A" })`.
