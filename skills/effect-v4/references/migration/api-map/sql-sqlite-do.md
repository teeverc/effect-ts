# v3 → v4 API map: @effect/sql-sqlite-do/SqliteClient

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql-sqlite-do/SqliteClient`

- `SqliteClient.SqliteClientConfig` -> `@effect/sql-sqlite-do/SqliteClient#SqliteClientConfig`: Retained; db is optional and storage may be supplied, but one of db or storage is required at runtime.
