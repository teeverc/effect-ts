# v3 → v4 API map: @effect/sql-sqlite-node/SqliteClient

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql-sqlite-node/SqliteClient`

- `SqliteClient.SqliteClient` -> `@effect/sql-sqlite-node/SqliteClient#SqliteClient`: Retained on node:sqlite, but the byte-export member was removed; use backup(destination) for file backup.

- `SqliteClient.SqliteClientConfig` -> `@effect/sql-sqlite-node/SqliteClient#SqliteClientConfig`: Retained; prepareCacheTTL now uses Duration.Input.

- `SqliteClient.layer` -> `SqliteClient.layer`: The layer now has a SqlError error channel for database opening and initialization failures. Handle setup failures, or use Layer.orDie when they should remain fatal.

- `SqliteClient.layerConfig` -> `SqliteClient.layerConfig`: The layer now fails with ConfigError | SqlError: configuration errors remain typed, and database opening and initialization failures are also typed. Handle SqlError as well, or use Layer.orDie if all setup failures should be fatal.

- `SqliteClient.make` -> `SqliteClient.make`: Database opening and initialization now fail with SqlError instead of defects. Handle the typed setup error, or use Effect.orDie when setup failures should remain fatal.
