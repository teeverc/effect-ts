# Configuration Advanced (schemas, providers, secrets, layers from config)

Basics (constructors, combinators, providing a source): `references/configuration.md`.

## Structured config with Config.schema

`Config.schema(codec, path?)` decodes any Schema whose canonical string-tree encoding has a concrete shape (scalar, struct, array, record, union). Scalars are decoded from strings, so `Schema.Int`, `Schema.Boolean`, `Schema.URL`, `Schema.Duration*` etc. work directly.

```ts
import { Config, ConfigProvider, Effect, Schema } from "effect"

const Database = Config.schema(
  Schema.Struct({
    host: Schema.String,
    port: Schema.Int.check(Schema.isBetween({ minimum: 1, maximum: 65535 })),
    pool: Schema.optional(Schema.Struct({ min: Schema.Int, max: Schema.Int }))
  }),
  "database"
)

// Object source
Database.parse(ConfigProvider.fromUnknown({ database: { host: "db", port: "5432" } }))
// Env source: DATABASE_HOST / DATABASE_PORT / DATABASE_POOL_MIN ...
Database.parse(ConfigProvider.fromEnv().pipe(ConfigProvider.constantCase))
```

- `Schema.Struct` vs `Config.all`: a struct validates the object **as a whole** (an existing-but-incomplete object is a validation error); `Config.all` combines independent lookups (missing child → the group is absent, so a group `withDefault` applies).
- Prefer per-field defaults (`Schema.optionalKey` + `Schema.withDecodingDefault`, or `Config.withDefault` on individual configs) over a group default.
- Opaque schemas (`Schema.Unknown`, `Schema.Any`, `Schema.Json`, `Schema.ObjectKeyword`) are rejected at construction. To read JSON from one variable: `Config.schema(Schema.fromJsonString(MySchema), "SETTINGS_JSON")`.
- Plain `Schema.Array`/`Schema.Record` read structural input only; use `Config.Array` / `Config.Record` to also accept `"a,b,c"` / `"k=v,k2=v2"`.
- Validation (v3 `Config.validate`): put `.check(...)` on the schema. Brands: `Config.schema(Schema.String.pipe(Schema.brand("UserId")), "USER_ID")`.

## Secrets with Redacted

```ts
import { Config, Effect, Redacted } from "effect"

const program = Effect.gen(function*() {
  const apiKey = yield* Config.Redacted("API_KEY")         // Redacted<string>
  yield* Effect.log(`key=${apiKey}`)                         // logs "key=<redacted>"
  const raw: string = Redacted.value(apiKey)                 // unwrap only at the call site that needs it
})

// Non-string secrets
const pin = Config.Int("PIN").pipe(Config.map((n) => Redacted.make(n, { label: "PIN" })))
```

- `Redacted.make(value, { label? })`, `Redacted.value`, `Redacted.isRedacted`, `Redacted.wipeUnsafe` (drop the stored value), `Redacted.makeEquivalence`.
- In schemas: `Schema.Redacted(inner)` (input already `Redacted`) vs `Schema.RedactedFromValue(inner)` (wraps a raw decoded value). v3 `Secret` is gone.

## Building layers from config (Layer.unwrap)

Choose an implementation at startup (from `references/examples/01_effect/03_services/20_layer-unwrap.ts`):

```ts
import { Config, Effect, Layer } from "effect"

export const MessageStoreLive = Layer.unwrap(
  Effect.gen(function*() {
    const useInMemory = yield* Config.Boolean("MESSAGE_STORE_IN_MEMORY").pipe(Config.withDefault(false))
    if (useInMemory) return MessageStore.layerInMemory
    const url = yield* Config.URL("MESSAGE_STORE_URL")
    return MessageStore.layerRemote(url)
  })
)
```

Or read config inside `Layer.effect` (see `references/configuration.md`). Config errors surface as layer build failures; `NodeRuntime.runMain` reports them. Use `Layer.orDie` if a missing config should be fatal rather than typed.

### Library options: Config.Wrap / Config.unwrap

```ts
import { Config } from "effect"

interface ClientOptions { readonly baseUrl: string; readonly timeoutMs: number }

// Callers pass either Config<ClientOptions> or { baseUrl: Config<string>, timeoutMs: Config<number> }
export const layerConfig = (options: Config.Wrap<ClientOptions>) =>
  Layer.effect(Client, Effect.flatMap(Config.unwrap(options), makeClient))
```

## Composing providers

```ts
import { ConfigProvider, Effect, Layer } from "effect"

// Env first, then defaults (fallback consulted only when a path is missing)
const provider = ConfigProvider.fromEnv().pipe(
  ConfigProvider.orElse(ConfigProvider.fromUnknown({ PORT: "3000", LOG_LEVEL: "Info" }))
)

// As layers on top of the current provider
const WithDefaults = ConfigProvider.layerAdd(ConfigProvider.fromUnknown({ PORT: "3000" }))       // fallback
const WithOverrides = ConfigProvider.layerAdd(overrides, { asPrimary: true })                    // primary

// .env file (needs FileSystem, e.g. NodeServices.layer from @effect/platform-node)
const DotEnv = ConfigProvider.layer(ConfigProvider.fromDotEnv({ path: ".env", expandVariables: true }))

// Mounted secrets directory (each file = one value)
const Secrets = ConfigProvider.layerAdd(ConfigProvider.fromDir({ rootPath: "/var/run/secrets/app" }))

// Path transforms
ConfigProvider.nested(provider, "APP")             // all lookups under APP_...
ConfigProvider.constantCase(provider)              // camelCase paths -> SCREAMING_SNAKE
ConfigProvider.mapInput(provider, (path) => path.map((s) => typeof s === "string" ? s.toLowerCase() : s))
```

Custom store:

```ts
const fromMap = (data: Record<string, string>) =>
  ConfigProvider.make((path) =>
    Effect.succeed(data[path.join(".")] !== undefined ? ConfigProvider.makeValue(data[path.join(".")]) : undefined)
  )
// return undefined for "missing"; fail with new ConfigProvider.SourceError({ message, cause }) only when the source is unreadable
```

Nodes: `makeValue(string)`, `makeRecord(keys, value?)`, `makeArray(length, value?)`.

## Testing

```ts
import { Config, ConfigProvider, Effect } from "effect"
import { it } from "@effect/vitest"

it.effect("reads port", () =>
  Effect.gen(function*() {
    const port = yield* Config.Port("PORT")
    // ...
  }).pipe(Effect.provide(ConfigProvider.layer(ConfigProvider.fromUnknown({ PORT: 8080 }))))
)

// Or without the runtime: Effect.runSync(Config.Port("PORT").parse(ConfigProvider.fromUnknown({ PORT: 8080 })))
```

## Errors

`Config.ConfigError` has `cause: ConfigProvider.SourceError | Schema.SchemaError`. Inspect with `error.cause instanceof ConfigProvider.SourceError` or `Schema.isSchemaError(error.cause)`; the issue path is included in the message. Absence is not a separate error class; handle it with `withDefault` / `option`.

## Pitfalls

- `withDefault` does not apply when a value is present but invalid, and a schema that decodes `undefined` successfully (e.g. `Schema.UndefinedOr`) never falls back to the default.
- `fromEnv` paths are case-sensitive; forgetting `constantCase` when using camelCase keys.
- `layerAdd` (fallback) vs `layer` (replace) vs `layerAdd(..., { asPrimary: true })` (override).
- Unwrapping `Redacted` early and passing plain strings around.
- Empty env values are treated as missing by default; pass `{ preserveEmptyStrings: true }` if `""` is meaningful.
