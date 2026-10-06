# v3 → v4 API map: @effect/sql-clickhouse/ClickhouseClient, @effect/sql-clickhouse/ClickhouseMigrator

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql-clickhouse/ClickhouseClient`

- `ClickhouseClient.ClickhouseClient` -> `@effect/sql-clickhouse/ClickhouseClient#ClickhouseClient`: Retained; the service value is now a Context.Service rather than a GenericTag.

- `ClickhouseClient.currentClickhouseSettings` -> `@effect/sql-clickhouse/ClickhouseClient#ClickhouseSettings`: Renamed and changed from FiberRef to Context.Reference; prefer client.withClickhouseSettings or provide the reference as a service.

- `ClickhouseClient.currentClientMethod` -> `@effect/sql-clickhouse/ClickhouseClient#ClientMethod`: Renamed and changed from FiberRef to Context.Reference; prefer client.asCommand or provide the reference as a service.

- `ClickhouseClient.currentQueryId` -> `@effect/sql-clickhouse/ClickhouseClient#QueryId`: Renamed and changed from FiberRef to Context.Reference; prefer client.withQueryId or provide the reference as a service.

## `@effect/sql-clickhouse/ClickhouseMigrator`

- `ClickhouseMigrator.MigrationError` -> `@effect/sql-clickhouse/ClickhouseMigrator#MigrationError`: Retained via effect/sql/Migrator; migrate reason and its lowercase values to kind with PascalCase values.
