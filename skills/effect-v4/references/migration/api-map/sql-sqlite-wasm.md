# v3 → v4 API map: @effect/sql-sqlite-wasm/OpfsWorker, @effect/sql-sqlite-wasm/SqliteClient

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql-sqlite-wasm/OpfsWorker`

- `OpfsWorker.OpfsWorkerConfig`: TODO: needs guidance

## `@effect/sql-sqlite-wasm/SqliteClient`

- `SqliteClient.SqliteClient` -> `@effect/sql-sqlite-wasm/SqliteClient#SqliteClient`: Retained with the same export/import surface; the service value is now a Context.Service.

- `SqliteClient.currentTransferables` -> `@effect/sql-sqlite-wasm/SqliteClient#Transferables`: Renamed and changed from FiberRef to Context.Reference; prefer withTransferables or provide Transferables as a service.
