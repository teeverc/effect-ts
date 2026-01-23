# Configuration Advanced

Use this guide when config needs structure, secrets, or test overrides.

- Use `Config.nested` to model nested configuration trees.
- Use `Config.redacted` for sensitive values to avoid leaking secrets.
- Use `ConfigProvider.fromMap` for test fixtures or local overrides.
- Use `Effect.withConfigProvider` to scope a provider for a specific effect.
- Prefer typed config and fail fast at startup.

## Example

```ts
import { Config, ConfigProvider } from "effect"

const hostPort = Config.all({
  host: Config.string("host"),
  port: Config.integer("port")
})

const service = Config.all({
  hostPort: hostPort.pipe(Config.nested("hostPort")),
  timeout: Config.integer("timeout")
})

const provider = ConfigProvider.fromMap(new Map([
  ["hostPort.host", "localhost"],
  ["hostPort.port", "5432"],
  ["timeout", "30"]
]))

const program = provider.load(service)
```
