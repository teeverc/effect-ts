# SQL (`effect/sql` + `@effect/sql-*` drivers)

Use this guide for database access: queries, models, repositories, batching resolvers, transactions, and migrations. Official example: `references/examples/40_sql/10_basics.ts`. The `examples/51_http-server/fixtures` show the same `Model.Class` reused for an HTTP API.

```ts
import { Migrator, SqlClient, SqlError, SqlModel, SqlResolver, SqlSchema, Statement } from "effect/sql"
import { Model } from "effect/schema"            // Model.Class lives in effect/schema (not effect/sql)
import { SqliteClient, SqliteMigrator } from "@effect/sql-sqlite-node"
```

`@effect/sql` was merged into `effect` (`effect/sql`). Drivers remain separate packages and must share the `effect` version. Most APIs are `@stability unstable`. Driver-specific options and clients are explicitly unstable.

## Drivers

| Package | Modules | Notes |
|---|---|---|
| `@effect/sql-pg` | `PgClient`, `PgMigrator`, `PgPool`, `PgConnection`, `PgTypes`, `PgAuth` | Built-in PostgreSQL client with pipelining, prepared statements, and binary codecs. No `pg` dependency (v4). |
| `@effect/sql-sqlite-node` | `SqliteClient`, `SqliteMigrator` | Uses `node:sqlite` and requires Node ≥ 22.16. |
| `@effect/sql-sqlite-bun`, `-sqlite-wasm`, `-sqlite-react-native`, `-sqlite-do` (Durable Objects) | `SqliteClient`, `SqliteMigrator` | |
| `@effect/sql-mysql2` | `MysqlClient`, `MysqlMigrator` | |
| `@effect/sql-mssql` | `MssqlClient`, `MssqlMigrator`, `Parameter`, `Procedure` | |
| `@effect/sql-libsql`, `-pglite`, `-clickhouse`, `-d1` | `LibsqlClient`, `PgliteClient`, `ClickhouseClient`, `D1Client` (+ migrators except D1) | `pglite` is new in v4. |

`@effect/sql-drizzle` was removed. Use Drizzle's own Effect integrations (`drizzle-orm/effect-postgres`, ...); see `references/migration/api-map/sql-drizzle.md`.

Each client exposes `layer(config)`, which provides `XClient | SqlClient.SqlClient`, and `layerConfig(Config.Wrap<config>)`.

```ts
const SqlLive = PgClient.layerConfig({
  url: Config.Redacted("DATABASE_URL"),
  transformQueryNames: Config.succeed(String.camelToSnake),   // `String` from "effect"
  transformResultNames: Config.succeed(String.snakeToCamel)
})
// or SqliteClient.layer({ filename: "app.db" })
```

## Queries with the `sql` tag

```ts
const program = Effect.gen(function*() {
  const sql = yield* SqlClient.SqlClient
  const rows = yield* sql<{ id: number; name: string }>`SELECT id, name FROM users WHERE id = ${id}`  // parameterized
  yield* sql`INSERT INTO users ${sql.insert({ name: "Ada", email: "ada@x.dev" })}`
  yield* sql`INSERT INTO users ${sql.insert([{ name: "a" }, { name: "b" }]).returning("*")}`
  yield* sql`UPDATE users SET ${sql.update(user, ["id"])} WHERE id = ${user.id}`  // 2nd arg: keys to omit
  yield* sql`SELECT * FROM users WHERE id IN ${sql.in(ids)}`
  yield* sql`SELECT * FROM ${sql("users")} WHERE ${sql.and([sql`active = ${true}`, sql`age > ${18}`])}`  // sql("x") = identifier
  const ddl = sql.onDialectOrElse({ pg: () => sql`...`, orElse: () => sql`...` })
})
```

- A statement is an `Effect<ReadonlyArray<Row>, SqlError>`. It also has:
  - `.stream`, a `Stream`
  - `.values`, rows as arrays
  - `.raw`, the driver result
  - `.unprepared`, which skips prepared statements
  - `.withoutTransform`, which skips name transforms
- Helpers: `sql.insert`, `sql.update`, `sql.updateValues(rows, alias)`, `sql.in`, `sql.and`, `sql.or`, `sql.csv`, `sql.join`, `sql.literal` (raw SQL fragment), `sql.unsafe(rawSql, params)`, `sql.onDialect({ sqlite, pg, mysql, mssql, clickhouse })`.
- PostgreSQL (`PgClient`):
  - `sql.json(value)` for JSON parameters. Plain objects are no longer inferred as JSON.
  - `listen(channel)` returns a scoped `Queue.Dequeue<string>`.
  - `notify(channel, payload)`.
  - Query strings must contain a single statement.

## Transactions and connections

```ts
const transfer = Effect.gen(function*() {
  yield* sql`UPDATE accounts SET balance = balance - ${amt} WHERE id = ${from}`
  yield* sql`UPDATE accounts SET balance = balance + ${amt} WHERE id = ${to}`
}).pipe(sql.withTransaction)   // nested calls use savepoints; any failure rolls back
```

- `sql.reserve` is a scoped dedicated connection.
- `sql.reactive(keys, query)` re-runs the query as a `Stream` when `Reactivity` invalidates those keys.
- `SqlClient.SafeIntegers` is a reference that opts in to bigint-safe integers where the driver supports it.

## Errors

`SqlError` (`_tag: "SqlError"`) has a structured `reason` (new in v4):

- `ConnectionError`, `AuthenticationError`, `AuthorizationError`, `SqlSyntaxError`
- `UniqueViolation` (has `constraint`), `ConstraintError`
- `DeadlockError`, `SerializationError`, `LockTimeoutError`, `StatementTimeoutError`
- `UnknownError`

Every reason has `cause`, `message?`, and `operation?`. `error.isRetryable` tells you whether the error is worth retrying.

```ts
const createUser = (input: typeof User.insert.Type) =>
  repo.insert(input).pipe(
    Effect.catchReason("SqlError", "UniqueViolation", () => Effect.fail(new EmailTaken())),
    Effect.retry({ while: (e) => SqlError.isSqlError(e) && e.isRetryable, times: 3 })
  )
```

`SqlError.ResultLengthMismatch` comes from `SqlResolver.ordered`.

## Models (`Model.Class`)

One field declaration derives six variants: `select` (the class itself), `insert`, `update`, `json`, `jsonCreate`, and `jsonUpdate`.

```ts
export const GroupId = Schema.String.pipe(Schema.brand("GroupId"))

export class Group extends Model.Class<Group>("Group")({
  id: Model.UuidV4Insert(GroupId),                       // app-generated on insert; not in json create/update
  name: Schema.NonEmptyString,
  slug: Schema.NonEmptyString.pipe(Model.FieldExcept(["update", "jsonUpdate"])),
  notes: Schema.NullOr(Schema.String).pipe(Model.FieldOnly(["select", "insert"])),  // DB-only
  memberCount: Model.Field({ select: Schema.Int, json: Schema.Int }),            // per-variant control
  createdAt: Model.DateTimeInsert,                       // set on insert
  updatedAt: Model.DateTimeUpdate                        // refreshed on update
}) {}

const row = yield* Group.insert.makeEffect({ name, slug, notes: null })  // fills id and timestamps via Clock
```

- Field helpers:
  - IDs: `GeneratedByDb`, `GeneratedByApp`, `UuidV4Insert`, `UuidV7Insert`, `UuidV4WithGenerate`, `UuidV7WithGenerate`, `UuidV4BytesInsert`
  - Timestamps: `DateTimeInsert` / `DateTimeUpdate` (plus `FromDate` and `FromNumber` variants for drivers that return `Date` or epoch numbers), `DateTimeWithNow`, `DateWithNow`
  - Encodings: `BooleanSqlite`, `JsonFromString`, `Uint8Array`
  - Other: `Sensitive` (excluded from JSON variants), `FieldOption` (optional everywhere: nullable in DB variants, a missing key allowed in JSON variants, decoded as `Option`), `optionalOption`
  - Generic combinators: `Field`, `FieldOnly`, `FieldExcept`, `fieldEvolve`, `Override`
- `Model.Generated` became `Model.GeneratedByDb` (read-only, `select`/`json` variants only). `Model.DateTimeFromDate` moved to `Schema.DateTimeUtcFromDate`. See `references/migration/api-map/sql.md`.

## Repositories and typed queries

```ts
const repo = yield* SqlModel.makeRepository(Group, { tableName: "groups", spanPrefix: "Groups", idColumn: "id" })
// repo.insert / insertVoid / update / updateVoid / findById (fails NoSuchElementError) / delete
// optional softDeleteColumn: reads skip deleted rows; delete updates the column instead

const findBySlug = SqlSchema.findOneOption({
  Request: Schema.String,
  Result: Group,
  execute: (slug) => sql`SELECT * FROM groups WHERE slug = ${slug}`
})
```

- `SqlSchema` functions: `findAll`, `findNonEmpty` (fails `NoSuchElementError` when empty), `findOne` (fails `NoSuchElementError`), `findOneOption`, `void` (encodes the request, ignores the result).
- Batching resolvers (combine with `Effect.all({ concurrency })` / `Effect.forEach`):
  - `SqlResolver.ordered({ Request, Result, execute })` — one result per request, in order
  - `SqlResolver.grouped({ Request, RequestGroupKey, Result, ResultGroupKey, execute })`
  - `SqlResolver.findById({ Id, Result, ResultId, execute })`
  - Run with `SqlResolver.request(resolver)(payload)`
  - `SqlModel.makeResolvers(Model, {...})` derives batched `insert`, `insertVoid`, `findById`, and `delete` resolvers
- Resolvers respect the current transaction connection.

## Migrations

```ts
const MigratorLive = SqliteMigrator.layer({
  loader: SqliteMigrator.fromRecord({
    "0001_create_groups": Effect.gen(function*() {
      const sql = yield* SqlClient.SqlClient
      yield* sql`CREATE TABLE groups (id TEXT PRIMARY KEY, name TEXT NOT NULL)`
    })
  })
  // or: loader: SqliteMigrator.fromFileSystem("./migrations")  // files `0001_name.ts` default-exporting an Effect
  // table?: "effect_sql_migrations", schemaDirectory?: dumps the schema after migrating (PgMigrator shells out via ChildProcessSpawner)
})
const SqlLive = MigratorLive.pipe(Layer.provideMerge(SqliteClient.layer({ filename: "app.db" })))
```

- Each driver migrator re-exports `effect/sql/Migrator` (`fromRecord`, `fromFileSystem`, `fromGlob`, `fromBabelGlob`) and adds `run` and `layer`.
- `PgMigrator.layer` also requires `ChildProcessSpawner | FileSystem | Path`. Provide `NodeServices.layer`.
- Migrations run once, in id order. Failures surface as `Migrator.MigrationError`.

## Wiring guidance

- Hide SQL behind a domain service (`Context.Service`) and provide `SqlLive` inside that service's layer, as the official example does. The rest of the app depends on `Groups`, not `SqlClient`.
- Convert `SqlError` / `SchemaError` to defects (`Effect.orDie`) or domain errors at the service boundary. Expose only domain errors.
- `KeyValueStore.layerSql`, persisted queues, `RateLimiter` stores, cluster `SqlMessageStorage`, and the workflow engine all run on top of a `SqlClient`.

## v3 → v4

- `@effect/sql` → `effect/sql`. `@effect/sql/Model` → `effect/schema` `Model`. `effect/unstable/sql` no longer exists.
- `SqlError` gained structured reasons.
- `@effect/sql-pg` no longer wraps `pg`. `PgClient.listen` returns a `Dequeue`, and `types` takes a `PgTypes.Registry`.
- See `references/migration/api-map/sql.md` and `sql-pg.md`.
