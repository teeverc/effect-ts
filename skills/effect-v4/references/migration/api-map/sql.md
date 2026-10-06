# v3 → v4 API map: @effect/sql (10 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql/Model`

- `Model.Any` -> `effect/schema/Model#Any`: Moved; v4 schemas track DecodingServices and EncodingServices separately instead of one Context type.

- `Model.AnyNoContext` -> `effect/schema/Model#Any`: The distinct no-context alias was removed; Model.Any propagates decoding and encoding services. Constrain both service types to never when required.

- `Model.BooleanFromNumber` -> `effect/Schema#BooleanFromBit`: Use the core 0 | 1 to boolean schema; Model.BooleanSqlite is the ready-made model field.

- `Model.Class` -> `effect/schema/Model#Class`: Moved; model variants remain select, insert, update, json, jsonCreate, and jsonUpdate.

- `Model.Date` -> `effect/schema/Model#Date`: Moved; still serializes DateTime.Utc as a YYYY-MM-DD string.

- `Model.DateTimeFromDate` -> `effect/Schema#DateTimeUtcFromDate`: Moved to core Schema and retains Date to DateTime.Utc conversion.

- `Model.Generated` -> `effect/schema/Model#GeneratedByDb`: Renamed and now read-only, with select and json variants only. Use Model.Field with select, update, and json to preserve writable v3 behavior.

- `Model.extract` -> `effect/schema/Model#extract`: Retained after moving the model variant helpers into core Effect's unstable schema package.

- `Model.fieldFromKey` -> `effect/Schema#encodeKeys`: The field helper was removed; apply encodeKeys to each concrete struct or model-variant schema that crosses the naming boundary.

- `Model.fields` -> `effect/schema/Model#fields`: Moved with the variant-model helpers into core Effect's unstable schema package.

- `Model.makeDataLoaders` -> `effect/sql/SqlModel#makeResolvers`: Returns RequestResolvers instead of callable loaders; execute with SqlResolver.request and use RequestResolver delay/batch combinators for batching controls. The insert resolver requires model decoding services as well as insert-schema encoding services.

## `@effect/sql/SqlClient`

- `SqlClient.SafeIntegers` -> `effect/sql/SqlClient#SafeIntegers`: Moved and changed from a Reference subclass to a Context.Reference value; provide it as a service.

- `SqlClient.TransactionConnection` -> `effect/sql/SqlClient#TransactionConnection`: Now a factory keyed by client id, not a singleton tag. Prefer the client's transactionService; the payload type is TransactionConnection.Service.

- `SqlClient.TypeId` -> `none`: The brand is private in v4; do not inspect or attach it, and obtain clients through SqlClient or SqlClient.make.

- `SqlClient.make` -> `effect/sql/SqlClient#make`: Moved; custom clients rename MakeOptions.reactiveMailbox to reactiveQueue and may supply transactionService.

## `@effect/sql/SqlConnection`

- `SqlConnection.Connection` -> `effect/sql/SqlConnection#Connection`: Moved; Connection.Acquirer is now top-level SqlConnection.Acquirer, and custom connections must implement executeValuesUnprepared.

## `@effect/sql/SqlError`

- `SqlError.SqlErrorTypeId` -> `effect/sql/SqlError#isSqlError`: The type id is private; use isSqlError for runtime narrowing or isSqlErrorReason for structured reason values.

## `@effect/sql/SqlEventJournal`

- `SqlEventJournal.layer` -> `effect/eventlog/SqlEventJournal#layer`: Moved; rename the eventLogTable layer option to entryTable.

- `SqlEventJournal.make` -> `effect/eventlog/SqlEventJournal#make`: Moved with the same entryTable and remotesTable options.

## `@effect/sql/SqlEventLogServer`

- `SqlEventLogServer.layerStorage` -> `effect/eventlog/SqlEventLogServerEncrypted#layerStorage`: Moved to the encrypted server module with the same options and EventLogEncryption requirement.

- `SqlEventLogServer.makeStorage` -> `effect/eventlog/SqlEventLogServerEncrypted#makeStorage`: Moved to the encrypted server module with the same SQL, encryption, and scope requirements.

## `@effect/sql/SqlPersistedQueue`

- `SqlPersistedQueue.layerStore` -> `effect/persistence/PersistedQueue#layerStoreSql`: Moved into PersistedQueue and renamed with the Sql suffix; options are unchanged.

- `SqlPersistedQueue.make` -> `effect/persistence/PersistedQueue#makeStoreSql`: Use the SQL store constructor; PersistedQueue.make creates a typed queue from a store factory and is not equivalent.

## `@effect/sql/SqlResolver`

- `SqlResolver.SqlResolver` -> `RequestResolver.RequestResolver<SqlResolver.SqlRequest<...>>`: The wrapper interface was removed; constructors return RequestResolvers. Execute them with effect/sql/SqlResolver#request.

- `SqlResolver.void` -> `effect/sql/SqlResolver#void`: Moved, but remove the leading tag and withContext arguments; it now returns a RequestResolver synchronously and runs through SqlResolver.request.

## `@effect/sql/SqlSchema`

- `SqlSchema.single` -> `effect/sql/SqlSchema#findOne`: Renamed with the same first-row-or-fail behavior; empty results use Cause.NoSuchElementError and schema failures use Schema.SchemaError.

- `SqlSchema.void` -> `effect/sql/SqlSchema#void`: Moved with the same encode, execute, and discard-result pattern; schema failures now use Schema.SchemaError.

## `@effect/sql/Statement`

- `Statement.FragmentId` -> `none`: The v4 fragment brand is private; use Fragment, fragment, and isFragment instead of direct type-id access.

- `Statement.Statement` -> `effect/sql/Statement#Statement`: Moved; the nested Transformer type is now top-level and its callback receives Fiber.Fiber rather than FiberRefs.FiberRefs.

- `Statement.currentTransformer` -> `effect/sql/Statement#CurrentTransformer`: Capitalized and changed from FiberRef\<Option\<Transformer\>\> to Context.Reference\<Transformer | undefined\>.

- `Statement.custom` -> `effect/sql/Statement#custom`: Retained, but returns a Custom segment and uses paramA/paramB/paramC; wrap it with Statement.fragment when a Fragment is required.

- `Statement.defaultEscape` -> `effect/sql/Statement#defaultEscape`: Moved with the same signature.

- `Statement.join` -> `effect/sql/Statement#join`: Moved with the same empty, single, and multiple-clause behavior.

- `Statement.make` -> `effect/sql/Statement#make`: Moved with the same constructor inputs.

- `Statement.makeCompiler` -> `effect/sql/Statement#makeCompiler`: Moved to core Effect; the constructor options are exposed as Statement.CompilerOptions and retain the dialect-specific callbacks.

- `Statement.setTransformer` -> `Layer.succeed(Statement.CurrentTransformer, transformer)`: The helper was removed; provide the CurrentTransformer reference as a layer.

- `Statement.unsafeFragment` -> `Statement.fragment([Statement.literal(sql, params)])`: The helper was removed; construct the low-level fragment explicitly, or use the active constructor's sql.unsafe for an executable statement.

- `Statement.withTransformer` -> `Effect.provideService(Statement.CurrentTransformer, transformer)`: The helper was removed; locally provide the transformer reference around the effect.

- `Statement.withTransformerDisabled` -> `Effect.provideService(Statement.CurrentTransformer, undefined)`: The helper was removed; locally provide undefined for the transformer reference.
