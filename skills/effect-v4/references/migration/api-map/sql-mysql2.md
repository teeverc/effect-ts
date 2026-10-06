# v3 → v4 API map: @effect/sql-mysql2/MysqlClient

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql-mysql2/MysqlClient`

- `MysqlClient.MysqlClientConfig` -> `@effect/sql-mysql2/MysqlClient#MysqlClientConfig`: Retained; connectionTTL uses Duration.Input and v4 adds disablePreparedStatements.
