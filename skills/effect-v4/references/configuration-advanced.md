# Configuration Advanced

Use this guide when config needs structure, secrets, or test overrides.

## Mental model

- Nest config to keep naming consistent across providers.
- Redact secrets so they can be logged safely.
- Providers can be swapped per scope for tests.

## Patterns

- Use `Config.nested` to model trees.
- Use `Config.redacted` for secrets.
- Use `ConfigProvider.fromMap` for tests.

## Walkthrough: nested config with redacted secret

```ts
import { Config, ConfigProvider, Effect } from "effect"

const DatabaseConfig = Config.all({
  url: Config.string("URL"),
  password: Config.redacted("PASSWORD")
}).pipe(Config.nested("DB"))

const provider = ConfigProvider.fromMap(
  new Map([
    ["DB.URL", "postgres://localhost/app"],
    ["DB.PASSWORD", "secret"]
  ])
)

const program = DatabaseConfig.pipe(
  Effect.withConfigProvider(provider),
  Effect.map((config) => ({
    url: config.url,
    password: config.password
  }))
)
```

## Pitfalls

- Logging secrets without redaction.
- Mixing nested and flat keys inconsistently.

## Docs

- `https://effect.website/docs/additional-resources/api-reference/`
- `https://effect.website/docs/data-types/redacted/`
