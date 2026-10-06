# v3 → v4 API map: @effect/sql-drizzle/Mysql, @effect/sql-drizzle/Pg, @effect/sql-drizzle/Sqlite

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql-drizzle/Mysql`

- `Mysql.MysqlDrizzle` -> `drizzle-orm/effect-mysql2#EffectMysql2Database`: The service tag was removed; use the database type and define an application Context.Tag if service access is required.

- `Mysql.layer` -> `Layer.effect(AppDb, MysqlDrizzle.makeWithDefaults())`: The package was removed; import MysqlDrizzle from drizzle-orm/effect-mysql2, define an application service tag, and compose with MysqlClient.layer.

- `Mysql.layerWithConfig` -> `Layer.effect(AppDb, MysqlDrizzle.makeWithDefaults(config))`: The package was removed; use drizzle-orm/effect-mysql2, port config to EffectDrizzleMySqlConfig, and define an application service tag.

- `Mysql.make` -> `drizzle-orm/effect-mysql2#makeWithDefaults`: Use Drizzle's Effect 4 integration; it returns EffectMysql2Database and requires MysqlClient.

- `Mysql.makeWithConfig` -> `drizzle-orm/effect-mysql2#makeWithDefaults`: The constructor split was removed; use makeWithDefaults(config), or make(config) when explicitly providing logger and cache services.

## `@effect/sql-drizzle/Pg`

- `Pg.PgDrizzle` -> `drizzle-orm/effect-postgres#EffectPgDatabase`: The service tag was removed; use the database type and define an application Context.Tag if service access is required.

- `Pg.layer` -> `Layer.effect(AppDb, PgDrizzle.makeWithDefaults())`: The package was removed; import PgDrizzle from drizzle-orm/effect-postgres, define an application service tag, and compose with PgClient.layer.

- `Pg.layerWithConfig` -> `Layer.effect(AppDb, PgDrizzle.makeWithDefaults(config))`: The package was removed; use drizzle-orm/effect-postgres, port config to EffectDrizzlePgConfig, and define an application service tag.

- `Pg.make` -> `drizzle-orm/effect-postgres#makeWithDefaults`: Use Drizzle's Effect 4 integration; it returns EffectPgDatabase and requires PgClient.

- `Pg.makeWithConfig` -> `drizzle-orm/effect-postgres#makeWithDefaults`: The constructor split was removed; use makeWithDefaults(config), or make(config) when explicitly providing logger and cache services.

## `@effect/sql-drizzle/Sqlite`

- `Sqlite.SqliteDrizzle` -> `matching drizzle-orm Effect SQLite database type`: The generic service tag was removed; use the backend-specific database type and define an application Context.Tag if needed.

- `Sqlite.layer` -> `Layer.effect(AppDb, SqliteDrizzle.makeWithDefaults())`: The package was removed; select the matching drizzle-orm Effect backend module, define an application service tag, and compose with its SQL client layer.

- `Sqlite.layerWithConfig` -> `Layer.effect(AppDb, SqliteDrizzle.makeWithDefaults(config))`: Select the matching drizzle-orm Effect backend, port config to EffectDrizzleSQLiteConfig, and define an application service tag.

- `Sqlite.make` -> `drizzle-orm/effect-sqlite-node#makeWithDefaults`: SQLite integration is backend-specific; use the module matching sql-sqlite-node, -bun, -do, -wasm, libsql, or d1.

- `Sqlite.makeWithConfig` -> `matching drizzle-orm Effect SQLite module#makeWithDefaults`: The generic constructor was removed; select the concrete backend and use makeWithDefaults(config), or make(config) with explicit services.
