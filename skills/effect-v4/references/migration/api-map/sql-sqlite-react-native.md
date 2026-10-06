# v3 → v4 API map: @effect/sql-sqlite-react-native/SqliteClient

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql-sqlite-react-native/SqliteClient`

- `SqliteClient.asyncQuery` -> `@effect/sql-sqlite-react-native/SqliteClient#AsyncQuery`: Renamed and changed from FiberRef to Context.Reference; prefer withAsyncQuery or provide AsyncQuery as a service.
