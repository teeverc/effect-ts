# v3 → v4 API map: @effect/sql-d1/D1Client

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql-d1/D1Client`

- `D1Client.D1ClientConfig` -> `@effect/sql-d1/D1Client#D1ClientConfig`: Retained; prepareCacheTTL now uses Duration.Input.
