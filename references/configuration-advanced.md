# Configuration Advanced

Use this guide when config needs structure, secrets, or test overrides.

- Use `Config.nested` to model nested configuration trees.
- Use `Config.redacted` for sensitive values to avoid leaking secrets.
- Use `ConfigProvider.fromMap` for test fixtures or local overrides.
- Use `Effect.withConfigProvider` to scope a provider for a specific effect.
- Prefer typed config and fail fast at startup.
