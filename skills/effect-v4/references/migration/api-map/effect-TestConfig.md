# v3 → v4 API map: effect/TestConfig

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TestConfig`

- `TestConfig.TestConfig` -> `none`: There is no v4 TestConfig service. Move runner settings to Vitest and native Arbitrary check options, or define an application-specific Context.Reference if runtime access is needed.

- `TestConfig.make` -> `{ repeats, retries, samples, shrinks }`: The v3 constructor only returned its parameter object. The TestConfig service was removed; keep a plain object only for application-owned configuration.
