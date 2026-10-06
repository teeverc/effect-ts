# v3 → v4 API map: @effect/sql-pg/PgClient

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql-pg/PgClient`

- `PgClient.PgClient` -> `@effect/sql-pg/PgClient#PgClient`: Retained; the service value is now a Context.Service. The listen method returns a scoped Effect acquiring a Queue.Dequeue\<PgConnection.Notification, SqlError\>; consume it with Queue operations or Stream.fromQueue and read each notification's payload. Connection failures after registration fail the queue with the original SqlError; scope closure interrupts consumers.

- `PgClient.PgClientConfig` -> `@effect/sql-pg/PgClient#PgClientConfig / PgPoolConfig`: Use PgClientConfig for base settings and PgPoolConfig for make/layer; pool sizing, idle timeout, and connection TTL moved to PgPoolConfig. The types option takes a PgTypes.Registry rather than node-pg custom types. Timestamp and timestamptz results (including array elements) are Date values with millisecond precision; infinity, -infinity, and out-of-range values become invalid Dates. Numeric readers can use getTime() or register numeric codecs. Date parameters bind as timestamptz; use PgTypes.timestamp(value) to preserve UTC fields when writing a timestamp column instead of applying the session TimeZone.

- `PgClient.PgClientFromPoolOptions` -> `none`: The node-pg Pool wrapper options were removed with fromPool. Use PgClient.PgPoolConfig with PgClient.make or PgClient.layer.

- `PgClient.fromPool` -> `none`: Wrapping an existing node-pg Pool was removed with the native protocol client. Use PgClient.make or PgClient.layer with connection settings.

- `PgClient.layerFromPool` -> `PgClient.layer`: Wrapping an existing node-pg Pool was removed. Provide connection settings to PgClient.layer instead.
