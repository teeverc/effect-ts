# Configuration (Config and ConfigProvider)

Use this guide when loading or validating runtime configuration.

## Mental model

- `Config` describes structure and types.
- A `ConfigProvider` supplies values (env by default).
- Config is loaded by running effects.

## Patterns

- Use `Config.all` to build structured config.
- Use `Config.withDefault` for optional values.
- Use `Config.literals([...], key)` for literal-string configuration backed by `Schema.Literals`.
- Use `Config.schema(schema, key)` when config should be decoded by a schema.
- Use `ConfigProvider.fromEnv` or `fromMap` for overrides.
- Decode once near startup and provide typed config to services/layers.

## Walkthrough: structured config from env

```ts
import { Config, ConfigProvider, Duration, Effect } from "effect"

const AppConfig = Config.all({
  host: Config.string("HOST"),
  port: Config.integer("PORT").pipe(Config.withDefault(3000)),
  timeout: Config.duration("TIMEOUT").pipe(Config.withDefault(Duration.seconds(30)))
})

const provider = ConfigProvider.fromEnv()

const program = AppConfig.pipe(
  Effect.withConfigProvider(provider),
  Effect.tap((config) => Effect.log(`host=${config.host}`))
)
```

## Pitfalls

- Reading config inside libraries instead of at startup.
- Using untyped strings for structured config.
- Expecting `withDefault` to cover parse errors. It applies to missing data; current `Config.schema` also treats missing array values as missing so defaults can apply.
- Nested keys depend on provider delimiters (configure `fromEnv`/`fromMap` accordingly).
- Assuming old config path composition behavior; current providers have fixes for path composition and directory-backed lookup.

## Docs

- `https://effect.website/docs/additional-resources/api-reference/`
- `https://effect.website/docs/requirements-management/default-services/`
