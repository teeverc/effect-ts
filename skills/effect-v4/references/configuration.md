# Configuration (Config and ConfigProvider)

Use this guide to read typed configuration. Advanced topics (schemas, providers, secrets, layers from config, testing): `references/configuration-advanced.md`.

## Mental model

- `Config<T>` describes which keys to read and how to decode them. It **is an Effect** (`Effect<T, Config.ConfigError>`): `yield*` it inside `Effect.gen`.
- When yielded, it reads the active `ConfigProvider.ConfigProvider` (a `Context.Reference`, default `ConfigProvider.fromEnv()`).
- `config.parse(provider)` runs it against an explicit provider (useful in tests).
- Every Config is backed by a Schema (`Config.schema(codec, path?)`); the built-in constructors are shortcuts.
- **v4 rename:** constructors are PascalCase: `Config.String`, `Config.Int`, `Config.Redacted`, ... (v3 `Config.string`, `Config.integer`, `Config.redacted` no longer exist).

## Constructors (all `(name?: string)` unless noted)

| v4 | Type | Notes |
| --- | --- | --- |
| `Config.String` / `Config.NonEmptyString` | `string` | |
| `Config.Number` / `Config.Finite` / `Config.Int` | `number` | `Finite` rejects NaN/Infinity; v3 `integer` → `Int` |
| `Config.Boolean` | `boolean` | accepts true/false, yes/no, on/off, 1/0, y/n |
| `Config.Port` | `number` | integer 1–65535 |
| `Config.Duration` | `Duration` | `"10 seconds"`, `"500 millis"`, `"Infinity"` |
| `Config.ByteSize` | `ByteSize` | `"10 MB"`, `"1 GiB"` |
| `Config.URL` / `Config.Date` | `URL` / `Date` | |
| `Config.LogLevel` | `LogLevel` | `"Info"`, `"Debug"`, ... |
| `Config.Redacted` | `Redacted<string>` | v3 `secret`/`redacted` |
| `Config.Literal(lit, name?)` / `Config.Literals([...], name?)` | literal union | v3 `literal(...)` |
| `Config.Array(schema, name?, { separator? })` | `ReadonlyArray<T>` | structural or `"a,b,c"` |
| `Config.Record(keySchema, valueSchema, name?, { separator?, keyValueSeparator? })` | record | structural or `"k=v,k2=v2"` |
| `Config.schema(schema, path?)` | any schema type | path: `string` or `Array<string \| number>` |
| `Config.succeed(value)` / `Config.fail(error)` | | |

## Combinators

- `Config.all({ ... })` / `Config.all([...])` – struct or tuple of configs.
- `Config.withDefault(value)` – used only when the value is **absent**; invalid values still fail.
- `Config.option` – `Option<T>`, `None` when absent.
- `Config.orElse((error) => otherConfig)` – fallback on any `ConfigError` (absent or invalid).
- `Config.map(f)`, `Config.mapEffect(f)` (v3 `mapOrFail`/`mapAttempt`), `Config.flatMap(f)`.
- `Config.nested("PREFIX")` – scope keys under a prefix (env: `PREFIX_KEY`; objects: `{ PREFIX: { KEY } }`).
- `Config.unwrap(wrapped)` with `Config.Wrap<T>` – accept either a `Config<T>` or a record of configs.

## Walkthrough

```ts
import { Config, Context, Duration, Effect, Layer, Schema } from "effect"

const ServerConfig = Config.all({
  host: Config.String("HOST").pipe(Config.withDefault("0.0.0.0")),
  port: Config.Port("PORT").pipe(Config.withDefault(3000)),
  timeout: Config.Duration("TIMEOUT").pipe(Config.withDefault(Duration.seconds(30))),
  env: Config.Literals(["development", "staging", "production"], "APP_ENV"),
  apiKey: Config.Redacted("API_KEY"),
  allowedOrigins: Config.Array(Schema.String, "ALLOWED_ORIGINS").pipe(Config.withDefault([]))
})

// Read config once while building a service layer.
export class Server extends Context.Service<Server, { readonly port: number }>()("app/Server") {
  static readonly layer = Layer.effect(
    Server,
    Effect.gen(function*() {
      const config = yield* ServerConfig            // fails with Config.ConfigError
      yield* Effect.log(`listening on ${config.host}:${config.port}`)
      return Server.of({ port: config.port })
    })
  )
}
```

## Providing config sources

```ts
import { Config, ConfigProvider, Effect } from "effect"

// Replace the provider for a program
program.pipe(Effect.provideService(ConfigProvider.ConfigProvider, ConfigProvider.fromUnknown({ PORT: 8080 })))

// As a layer (application or tests)
const TestConfig = ConfigProvider.layer(ConfigProvider.fromUnknown({ HOST: "localhost", PORT: "8080" }))

// Run one config against a provider directly
const port = Config.Port("PORT").parse(ConfigProvider.fromEnv({ env: { PORT: "8080" } }))
```

Sources: `fromEnv({ env? })` (default; `process.env` + `import.meta.env`), `fromEnvRecord(record)`, `fromUnknown(object)`, `fromDotEnvContents(text)`, `fromDotEnv({ path? })` (needs `FileSystem`), `fromDir({ rootPath })` (Kubernetes-style mounted files; needs `FileSystem` + `Path`), `make(get)` for custom stores. There is no `fromMap`/`fromJson`/`Effect.withConfigProvider` in v4.

## Path mapping

- Env provider joins path segments with `_`: `Config.String("host").pipe(Config.nested("database"))` reads `database_host`. Env lookups are case-sensitive; apply `ConfigProvider.constantCase` to map `databaseHost` / nested paths to `DATABASE_HOST`.
- Object provider (`fromUnknown`) walks nested objects/arrays: `{ database: { host } }`.
- Empty strings are treated as **missing** by default (so `withDefault` applies). Pass `{ preserveEmptyStrings: true }` to a provider to keep them.

## v3 → v4

- PascalCase constructors (`string` → `String`, `integer` → `Int`, `secret`/`redacted` → `Redacted`, `literal` → `Literals`, `array`/`repeat` → `Array(schema, path)`, `hashMap` → `Config.schema(Schema.HashMap(...))`), `mapOrFail` → `mapEffect`, `validate` → `Config.schema(schema.check(...))`, `withDescription` → schema annotations, `zip` → `all([a, b])`.
- `ConfigProvider.fromMap`/`fromJson` → `fromUnknown`; `Effect.withConfigProvider`/`Layer.setConfigProvider` → `Effect.provideService(ConfigProvider.ConfigProvider, p)` / `ConfigProvider.layer(p)`.
- `ConfigError` is a single class with `cause: SourceError | SchemaError` (no `MissingData`/`InvalidData` tree).
- Full maps: `references/migration/api-map/effect-Config.md`, `effect-ConfigProvider.md`, `effect-ConfigError.md`.

## Pitfalls

- Using v3 lowercase constructors (`Config.string`) — they do not exist.
- Expecting `withDefault` to hide invalid values; it only covers absence. Use `orElse` for "invalid → fallback".
- `withDefault` on a whole `Config.all` group replaces the **entire** group if any child is absent; put defaults on individual fields.
- Reading config deep inside library code instead of once in a Layer.
- Logging secrets: use `Config.Redacted` (see `references/configuration-advanced.md`).
