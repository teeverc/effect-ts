# v3 → v4 API map: effect/GlobalValue

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/GlobalValue`

- `GlobalValue.globalValue` -> `module-scoped const`: The global registry helper was removed; use a module singleton, or explicitly own a globalThis and Symbol.for registry when cross-bundle identity is required.
