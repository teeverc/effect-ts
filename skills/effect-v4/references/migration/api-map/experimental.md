# v3 → v4 API map: @effect/experimental/DevTools (20 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/experimental/DevTools/Client`

- `Client.Client` -> `effect/devtools/DevToolsClient#DevToolsClient`: Client was renamed to the DevToolsClient Context.Service class.

- `Client.ClientImpl` -> `effect/devtools/DevToolsClient#DevToolsClient["Service"]`: Use the service shape from DevToolsClient; unsafeAddSpan was replaced by sendUnsafe.

- `Client.layer` -> `effect/devtools/DevToolsClient#layer`: Import layer from the v4 unstable DevToolsClient module.

- `Client.make` -> `effect/devtools/DevToolsClient#make`: Import make from the v4 unstable DevToolsClient module.

## `@effect/experimental/DevTools/Domain`

- `Domain.ExternalSpanFrom` -> `effect/Schema#Codec.Encoded<typeof effect/devtools/DevToolsSchema#ExternalSpan>`: The named encoded alias was removed; derive it with Schema.Codec.Encoded from ExternalSpan.

- `Domain.MetricFrom` -> `effect/Schema#Codec.Encoded<typeof effect/devtools/DevToolsSchema#Metric>`: The named encoded alias was removed; derive it with Schema.Codec.Encoded from Metric.

- `Domain.MetricsSnapshotFrom` -> `effect/Schema#Codec.Encoded<typeof effect/devtools/DevToolsSchema#MetricsSnapshot>`: The named encoded alias was removed; derive it with Schema.Codec.Encoded from MetricsSnapshot.

- `Domain.ParentSpanFrom` -> `effect/Schema#Codec.Encoded<typeof effect/devtools/DevToolsSchema#ParentSpan>`: The named encoded alias was removed; derive it with Schema.Codec.Encoded from ParentSpan.

- `Domain.SpanFrom` -> `effect/Schema#Codec.Encoded<typeof effect/devtools/DevToolsSchema#Span>`: The named encoded alias was removed; derive it with Schema.Codec.Encoded from Span.

- `Domain.metric` -> `none`: The metric schema helper is private in v4; use the exported Counter, Frequency, Gauge, Histogram, Summary, or Metric schemas, or build a Schema.Struct.

## `@effect/experimental/DevTools/Server`

- `Server.run` -> `effect/devtools/DevToolsServer#run`: Import run from the v4 unstable DevToolsServer module.

## `@effect/experimental/Event`

- `Event.Event.AddError` -> `effect/eventlog/Event#AddError`: This type moved from the Event namespace to a top-level export.

- `Event.Event.Any` -> `effect/eventlog/Event#Any`: This type moved from the Event namespace to a top-level export.

- `Event.Event.Context` -> `effect/eventlog/Event#Services`: Event schema context is now represented by decoding and encoding Services.

- `Event.Event.ContextWithTag` -> `effect/eventlog/Event#Services<effect/eventlog/Event#WithTag<Events, Tag>>`: Filter with WithTag and derive its decoding and encoding Services.

- `Event.Event.Error` -> `effect/eventlog/Event#Error`: This type moved from the Event namespace to a top-level export.

- `Event.Event.ErrorSchema` -> `effect/eventlog/Event#ErrorSchema`: This type moved from the Event namespace to a top-level export.

- `Event.Event.Payload` -> `effect/eventlog/Event#Payload`: This type moved from the Event namespace to a top-level export.

- `Event.Event.PayloadSchema` -> `effect/eventlog/Event#PayloadSchema`: This type moved from the Event namespace to a top-level export.

- `Event.Event.Success` -> `effect/eventlog/Event#Success`: This type moved from the Event namespace to a top-level export.

- `Event.Event.SuccessSchema` -> `effect/eventlog/Event#SuccessSchema`: This type moved from the Event namespace to a top-level export.

- `Event.Event.Tag` -> `effect/eventlog/Event#Tag`: This type moved from the Event namespace to a top-level export.

- `Event.Event.ToService` -> `effect/eventlog/Event#ToService`: This type moved from the Event namespace to a top-level export.

- `Event.TypeId` -> `effect/eventlog/Event#TypeId`: Import TypeId from the v4 unstable Event module; its runtime representation is now a string brand.

- `Event.make` -> `effect/eventlog/Event#make`: Import make from the v4 unstable Event module.

## `@effect/experimental/EventGroup`

- `EventGroup.EventGroup.Any` -> `effect/eventlog/EventGroup#Any`: This type moved from the EventGroup namespace to a top-level export.

- `EventGroup.EventGroup.AnyWithProps` -> `effect/eventlog/EventGroup#AnyWithProps`: This type moved from the EventGroup namespace to a top-level export.

- `EventGroup.EventGroup.Context` -> `effect/eventlog/EventGroup#ServicesClient | effect/eventlog/EventGroup#ServicesServer`: Choose the client or server schema services for the required direction.

- `EventGroup.EventGroup.ToService` -> `effect/eventlog/EventGroup#ToService`: This type moved from the EventGroup namespace to a top-level export.

- `EventGroup.TypeId` -> `effect/eventlog/EventGroup#TypeId`: Import TypeId from the v4 unstable EventGroup module; its runtime representation is now a string brand.

## `@effect/experimental/EventJournal`

- `EventJournal.EntryIdTypeId` -> `effect/eventlog/EventJournal#EntryIdTypeId`: Import EntryIdTypeId from the v4 EventJournal module; it is now a string brand.

- `EventJournal.ErrorTypeId` -> `none`: The v4 error marker is private; narrow with EventJournalError instead.

- `EventJournal.RemoteIdTypeId` -> `effect/eventlog/EventJournal#RemoteIdTypeId`: Import RemoteIdTypeId from the v4 EventJournal module; it is now a string brand.

- `EventJournal.makeEntryId` -> `effect/eventlog/EventJournal#makeEntryIdUnsafe`: The unchecked EntryId constructor was renamed to makeEntryIdUnsafe.

- `EventJournal.makeMemory` -> `effect/eventlog/EventJournal#makeMemory`: The in-memory constructor moved into core Effect and now returns the Context.Service implementation through its Service projection.

- `EventJournal.makeRemoteId` -> `effect/eventlog/EventJournal#makeRemoteIdUnsafe`: The unchecked RemoteId constructor was renamed to makeRemoteIdUnsafe.

## `@effect/experimental/EventLog`

- `EventLog.Handlers` -> `effect/eventlog/EventLog#Handlers`: Import Handlers from the v4 EventLog module; handlers now also receive storeId.

- `EventLog.HandlersTypeId` -> `effect/eventlog/EventLog#HandlersTypeId`: Import HandlersTypeId from the v4 EventLog module.

- `EventLog.SchemaTypeId` -> `effect/eventlog/EventLog#SchemaTypeId`: Import SchemaTypeId from the v4 EventLog module.

- `EventLog.group` -> `effect/eventlog/EventLog#group`: Import group from the v4 EventLog module; it now requires the shared Registry service.

- `EventLog.layer` -> `effect/eventlog/EventLog#layer`: The v4 layer takes both the schema and handler layer; use layerEventLog for runtime only.

- `EventLog.layerIdentityKvs` -> `none`: Compose KeyValueStore.toSchemaStore, EventLog.IdentitySchema, EventLog.makeIdentity, and Layer.effect manually.

## `@effect/experimental/EventLogRemote`

- `EventLogRemote.Ack` -> `none`: A write acknowledgement is now the void success of EventLogMessage.WriteSingleRpc or WriteChunkedRpc.

- `EventLogRemote.Changes` -> `effect/eventlog/EventLogMessage#ChangesRpc`: ChangesRpc replaces the separate request and response models with one streaming RPC.

- `EventLogRemote.EventLogRemote` -> `effect/eventlog/EventLogRemote#EventLogRemote`: Use the v4 Context.Service; methods now take storeId-aware options.

- `EventLogRemote.Hello` -> `effect/eventlog/EventLogMessage#HelloResponse`: HelloResponse replaces Hello and includes the v4 authentication challenge; HelloRpc defines the endpoint.

- `EventLogRemote.Pong` -> `none`: The event-log Pong model was removed; heartbeats belong to the generic RPC socket protocol.

- `EventLogRemote.ProtocolRequest` -> `effect/eventlog/EventLogMessage#EventLogRemoteRpcs`: EventLogRemoteRpcs and generic RPC serialization replace the old protocol request union.

- `EventLogRemote.ProtocolRequestMsgPack` -> `effect/rpc/RpcSerialization#layerSchemaBinary`: Use the generic SchemaBinary RPC serialization layer instead of a request-specific schema.

- `EventLogRemote.ProtocolResponse` -> `effect/eventlog/EventLogMessage#EventLogRemoteRpcs`: EventLogRemoteRpcs and generic RPC serialization replace the old protocol response union.

- `EventLogRemote.ProtocolResponseMsgPack` -> `effect/rpc/RpcSerialization#layerSchemaBinary`: Use the generic SchemaBinary RPC serialization layer instead of a response-specific schema.

- `EventLogRemote.RemoteAdditions` -> `none`: This unused protocol model has no v4 counterpart.

- `EventLogRemote.RequestChanges` -> `effect/eventlog/EventLogMessage#ChangesRpc`: ChangesRpc replaces the separate request model with one streaming RPC.

- `EventLogRemote.StopChanges` -> `none`: Interrupt the ChangesRpc stream instead of sending a StopChanges message.

- `EventLogRemote.decodeRequest` -> `effect/eventlog/EventLogMessage#EventLogRemoteRpcs`: Generic RPC framing and RpcSerialization.layerSchemaBinary replace the module-specific request decoder.

- `EventLogRemote.decodeResponse` -> `effect/eventlog/EventLogMessage#EventLogRemoteRpcs`: Generic RPC framing and RpcSerialization.layerSchemaBinary replace the module-specific response decoder.

- `EventLogRemote.encodeRequest` -> `effect/eventlog/EventLogMessage#EventLogRemoteRpcs`: Generic RPC framing and RpcSerialization.layerSchemaBinary replace the module-specific request encoder.

- `EventLogRemote.encodeResponse` -> `effect/eventlog/EventLogMessage#EventLogRemoteRpcs`: Generic RPC framing and RpcSerialization.layerSchemaBinary replace the module-specific response encoder.

- `EventLogRemote.fromSocket` -> `effect/eventlog/EventLogRemote#makeEncrypted + effect/rpc/RpcClient#makeProtocolSocket`: Construct the encrypted remote separately from its generic RPC socket protocol.

- `EventLogRemote.layerWebSocket` -> `effect/eventlog/EventLogRemote#layerEncrypted + effect/rpc/RpcClient#layerProtocolSocket`: Compose the encrypted remote with the generic socket protocol, SchemaBinary serialization, and a Socket provider.

- `EventLogRemote.layerWebSocketBrowser` -> `effect/eventlog/EventLogRemote#layerEncrypted + effect/rpc/RpcClient#layerProtocolSocket + @effect/platform-browser/BrowserSocket#layerWebSocket`: Compose the encrypted remote and generic RPC socket protocol with the browser WebSocket layer.

## `@effect/experimental/EventLogServer`

- `EventLogServer.Storage` -> `effect/eventlog/EventLogServerEncrypted#Storage`: Use the encrypted server Storage service, which is storeId- and session-aware.

- `EventLogServer.layerStorageMemory` -> `effect/eventlog/EventLogServerEncrypted#layerStorageMemory`: Use the encrypted server memory storage layer; storage is now storeId- and session-aware.

- `EventLogServer.makeHandler` -> `effect/eventlog/EventLogServerEncrypted#layer + effect/rpc/RpcServer#layerProtocolSocketServer`: Compose the encrypted server layer with the generic RPC socket server; there is no per-socket handler factory.

- `EventLogServer.makeHandlerHttp` -> `effect/eventlog/EventLogServerEncrypted#layer + effect/rpc/RpcServer#makeProtocolWithHttpEffectWebsocket`: Use the returned httpEffect for upgrades and provide its protocol to the encrypted server layer.

- `EventLogServer.makeStorageMemory` -> `effect/eventlog/EventLogServerEncrypted#makeStorageMemory`: Use the encrypted server memory storage constructor.

## `@effect/experimental/PersistedCache`

- `PersistedCache.make` -> `effect/persistence/PersistedCache#make`: Pass lookup as the first argument and options second; timeToLive now receives exit before request and the service is Persistence.Persistence.

## `@effect/experimental/PersistedQueue`

- `PersistedQueue.ErrorTypeId` -> `effect/persistence/PersistedQueue#ErrorTypeId`: Retained as a string brand; the runtime marker now uses the persistence module path.

- `PersistedQueue.TypeId` -> `effect/persistence/PersistedQueue#TypeId`: Import TypeId from the v4 unstable PersistedQueue module; it is now a string brand.

- `PersistedQueue.make` -> `effect/persistence/PersistedQueue#make`: Import make from the v4 unstable PersistedQueue module.

## `@effect/experimental/PersistedQueue/Redis`

- `Redis.layerStore` -> `effect/persistence/PersistedQueue#layerStoreRedis`: The Redis adapter was merged into PersistedQueue and now requires the generic Redis.Redis service.

- `Redis.layerStoreConfig` -> `none`: Configure a Redis provider such as NodeRedis.layerConfig separately, then compose it with PersistedQueue.layerStoreRedis.

- `Redis.make` -> `effect/persistence/PersistedQueue#makeStoreRedis`: The Redis adapter was merged into PersistedQueue and now requires the generic Redis.Redis service.

## `@effect/experimental/Persistence`

- `Persistence.BackingPersistence` -> `effect/persistence/Persistence#BackingPersistence`: Use the v4 BackingPersistence Context.Service class.

- `Persistence.BackingPersistenceTypeId` -> `none`: The BackingPersistence brand is no longer publicly exported in v4.

- `Persistence.ErrorTypeId` -> `none`: The v4 persistence error identifier is private; narrow with the exported error classes.

- `Persistence.PersistenceBackingError` -> `effect/persistence/Persistence#PersistenceError`: PersistenceError now represents failures from the backing persistence implementation.

- `Persistence.PersistenceError` -> `effect/persistence/Persistence#PersistenceError | effect/Schema#SchemaError`: The old combined alias was split into backing PersistenceError and schema SchemaError.

- `Persistence.PersistenceParseError` -> `effect/Schema#SchemaError`: Persistence parsing failures now use the core SchemaError type.

- `Persistence.ResultPersistence` -> `effect/persistence/Persistence#Persistence`: ResultPersistence was renamed to Persistence and is now a Context.Service class.

- `Persistence.ResultPersistence.Key` -> `effect/persistence/Persistable#Persistable`: Persistable is the v4 schema-backed persistence key contract.

- `Persistence.ResultPersistence.KeyAny` -> `effect/persistence/Persistable#Any`: Use Persistable.Any for an arbitrary v4 persistence key contract.

- `Persistence.ResultPersistence.TimeToLiveArgs` -> `Parameters<effect/persistence/Persistable#TimeToLiveFn<K>>`: Derive the tuple from TimeToLiveFn; its order is now exit then request.

- `Persistence.ResultPersistenceStore` -> `effect/persistence/Persistence#PersistenceStore`: ResultPersistenceStore was renamed to PersistenceStore.

- `Persistence.ResultPersistenceTypeId` -> `none`: The ResultPersistence brand is no longer publicly exported in v4.

- `Persistence.layerKeyValueStore` -> `effect/persistence/Persistence#layerBackingKvs`: The KeyValueStore backing layer was renamed to layerBackingKvs.

- `Persistence.layerMemory` -> `effect/persistence/Persistence#layerBackingMemory`: Use layerBackingMemory for the old backing service; v4 layerMemory creates the higher-level Persistence service.

- `Persistence.layerResult` -> `effect/persistence/Persistence#layer`: The ResultPersistence service layer was renamed to layer.

- `Persistence.layerResultKeyValueStore` -> `effect/persistence/Persistence#layerKvs`: The combined KeyValueStore-backed result layer was renamed to layerKvs.

- `Persistence.layerResultMemory` -> `effect/persistence/Persistence#layerMemory`: The combined memory-backed result layer was renamed to layerMemory.

## `@effect/experimental/Persistence/Redis`

- `Redis.layer` -> `effect/persistence/Persistence#layerBackingRedis`: The Redis backing adapter was merged into Persistence and now requires the generic Redis.Redis service.

- `Redis.layerConfig` -> `none`: Compose Persistence.layerBackingRedis with a config-driven provider such as NodeRedis.layerConfig.

- `Redis.layerResult` -> `effect/persistence/Persistence#layerRedis`: The combined Redis persistence layer was merged into Persistence and now requires Redis.Redis.

- `Redis.layerResultConfig` -> `none`: Compose Persistence.layerRedis with a config-driven provider such as NodeRedis.layerConfig.

- `Redis.make` -> `none`: V4 exposes Redis-backed layers over the Redis.Redis service, not a constructor that creates an ioredis client directly.

## `@effect/experimental/RateLimiter`

- `RateLimiter.ErrorTypeId` -> `effect/persistence/RateLimiter#ErrorTypeId`: Retained as a string brand; the runtime marker now uses the persistence module path.

- `RateLimiter.RateLimiterError` -> `effect/persistence/RateLimiter#RateLimiterError`: The retained name is now a wrapper error class whose reason is RateLimitExceeded or RateLimitStoreError.

- `RateLimiter.RateLimiterStore` -> `RateLimiter.RateLimiterStore`: Use the Context.Service class from effect/persistence/RateLimiter. Custom tokenBucket implementations must return [remaining, elapsedMillis] instead of a number, preserving fractional counts and elapsed refill time.

- `RateLimiter.TypeId` -> `effect/persistence/RateLimiter#TypeId`: Import TypeId from the v4 unstable RateLimiter module; it is now a string brand.

- `RateLimiter.makeSleep` -> `effect/persistence/RateLimiter#sleep`: The accessor Effect was replaced by sleep; obtain the RateLimiter service and pass it to sleep directly or with its curried overload.

## `@effect/experimental/RateLimiter/Redis`

- `Redis.layerStore` -> `effect/persistence/RateLimiter#layerStoreRedis`: The Redis adapter was merged into RateLimiter and now requires the generic Redis.Redis service.

- `Redis.layerStoreConfig` -> `effect/persistence/RateLimiter#layerStoreRedisConfig`: Use the merged Redis store config layer; connection configuration belongs to a separate Redis provider.

- `Redis.make` -> `effect/persistence/RateLimiter#makeStoreRedis`: The Redis adapter was merged into RateLimiter and now requires the generic Redis.Redis service.

## `@effect/experimental/Reactivity`

- `Reactivity.Reactivity` -> `effect/reactivity/Reactivity#Reactivity`: Use the Reactivity interface for implementations and the Reactivity Context.Service value as the service key. Implementations require the TypeId brand; prefer Reactivity.make. Unsafe methods were renamed with an Unsafe suffix.

- `Reactivity.Reactivity.Service` -> `effect/reactivity/Reactivity#Reactivity`: The named namespace member was removed; use the branded Reactivity interface directly. Prefer Reactivity.make, or include [Reactivity.TypeId]: Reactivity.TypeId in a custom implementation.

- `Reactivity.make` -> `effect/reactivity/Reactivity#make`: Import make from the v4 unstable Reactivity module.

- `Reactivity.query`: TODO: needs guidance

## `@effect/experimental/RequestResolver`

- `RequestResolver.PersistedRequest` -> `effect/Request#Request & effect/persistence/Persistable#Persistable`: Intersect a Request with Persistable or define it with Persistable.Class; there is no combined named export.

- `RequestResolver.PersistedRequest.Any` -> `effect/Request#Any & effect/persistence/Persistable#Any`: Intersect the Request and Persistable helper types for an arbitrary persisted request.

- `RequestResolver.dataLoader` -> `effect/RequestResolver#setDelay + effect/RequestResolver#batchN`: Pipe the resolver through setDelay(options.window) and batchN(options.maxBatchSize ?? Infinity); the transformation is now pure.

- `RequestResolver.persisted` -> `effect/RequestResolver#persisted`: Retained after moving to core RequestResolver; requests now implement Persistable and use Persistence.Persistence, timeToLive is optional, and staleWhileRevalidate is supported.

## `@effect/experimental/Sse`

- `Sse.RetryTypeId` -> `none`: The Retry identifier is private in v4; use effect/encoding/Sse#Retry and Retry.is instead of inspecting the brand.

## `@effect/experimental/VariantSchema`

- `VariantSchema.Extract` -> `effect/schema/VariantSchema#Extract`: Import the retained helper from the v4 module; its erased schema constraint is Schema.Top.

- `VariantSchema.Field.Any` -> `effect/schema/VariantSchema#Field.Any`: Import the retained Field.Any helper type from the v4 unstable VariantSchema module.

- `VariantSchema.Field.Config` -> `effect/schema/VariantSchema#Field.Config`: Import the retained Field.Config helper type from the v4 unstable VariantSchema module.

- `VariantSchema.Field.Fields` -> `effect/schema/VariantSchema#Field.Fields`: Import the retained Field.Fields helper type from the v4 unstable VariantSchema module.

- `VariantSchema.Field.ValueAny` -> `effect/Schema#Top`: Use the core Schema.Top constraint for an arbitrary field value schema.

- `VariantSchema.FieldTypeId` -> `none`: The Field brand is private in v4; use VariantSchema.isField for narrowing.

- `VariantSchema.Struct.Fields` -> `effect/schema/VariantSchema#Struct.Fields`: Import the retained Struct.Fields helper type from the v4 unstable VariantSchema module.

- `VariantSchema.TypeId` -> `effect/schema/VariantSchema#TypeId`: Use the retained runtime value; in type position use typeof VariantSchema.TypeId.

- `VariantSchema.fromKey` -> `none`: Field-level fromKey was not ported; for whole-struct encoded-key renaming consider Schema.encodeKeys.

- `VariantSchema.fromKey.Rename` -> `none`: The fromKey rename helper was not ported; for whole-struct encoded-key renaming consider Schema.encodeKeys.

- `VariantSchema.make` -> `effect/schema/VariantSchema#make`: Import make from the v4 module; FieldOnly and FieldExcept take one key array and Union takes one member array.
