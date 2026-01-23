# Configuration (Config and ConfigProvider)

Use this guide when loading or validating runtime configuration.

- `Config` describes the structure and requirements of the configuration your program needs.
- A `ConfigProvider` supplies the configuration values (the default provider reads from environment variables).
- `Config` values are loaded by running effects that use the active provider.
- Use `Effect.withConfigProvider` to override the provider for a scope of execution.
