# v3 → v4 API map: @effect/cluster (27 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/cluster/ClusterCron`

- `ClusterCron.make` -> `effect/cluster/ClusterCron#make`: Moved into core Effect. The constructor remains; Duration.DurationInput is now Duration.Input.

## `@effect/cluster/ClusterError`

- `ClusterError.TypeId` -> `none`: The shared marker is private in v4. Use the exported tagged error classes, their \_tag fields, or class-specific is guards.

## `@effect/cluster/ClusterSchema`

- `ClusterSchema.ClientTracingEnabled` -> `effect/cluster/ClusterSchema#ClientTracingEnabled`: Moved into core Effect and changed from a Context.Tag subclass to a Context.Reference value.

- `ClusterSchema.Persisted` -> `effect/cluster/ClusterSchema#Persisted`: Moved into core Effect and changed from a Context.Tag subclass to a Context.Reference value with the same false default.

- `ClusterSchema.ShardGroup` -> `effect/cluster/ClusterSchema#ShardGroup`: Moved into core Effect and changed from a Context.Tag subclass to a Context.Reference value.

- `ClusterSchema.Uninterruptible` -> `effect/cluster/ClusterSchema#Uninterruptible`: Now a Context.Reference value. Replace its static methods with ClusterSchema.isUninterruptibleForServer and isUninterruptibleForClient.

## `@effect/cluster/ClusterWorkflowEngine`

- `ClusterWorkflowEngine.layer` -> `effect/cluster/ClusterWorkflowEngine#layer`: Moved into core Effect with the same cluster-backed WorkflowEngine layer composition.

- `ClusterWorkflowEngine.make` -> `effect/cluster/ClusterWorkflowEngine#make`: Moved into core Effect; the constructor still uses Sharding and MessageStorage.

## `@effect/cluster/DeliverAt`

- `DeliverAt.symbol` -> `effect/cluster/DeliverAt#symbol`: Moved into core Effect; the protocol key is now the string literal \~effect/cluster/DeliverAt rather than a global symbol.

## `@effect/cluster/Entity`

- `Entity.HandlersFrom` -> `effect/cluster/Entity#HandlersFrom`: Moved into core Effect; handler results now use Rpc.WrapperOr, which accepts either the raw RPC result or its wrapper.

- `Entity.Replier.Success` -> `effect/cluster/Entity#Replier.Success`: Moved into core Effect; streaming replies may use Queue.Dequeue with Cause.Done instead of the removed Mailbox type.

- `Entity.TypeId` -> `none`: The entity marker is private in v4. Use Entity.isEntity for runtime refinement.

## `@effect/cluster/EntityAddress`

- `EntityAddress.EntityAddressFromSelf` -> `effect/cluster/EntityAddress#EntityAddress`: The separate self schema was removed; the v4 Schema.Class is itself the EntityAddress schema.

- `EntityAddress.TypeId` -> `none`: The marker is private in v4. Use the exported EntityAddress class and schema.

## `@effect/cluster/EntityProxy`

- `EntityProxy.ConvertHttpApi` -> `effect/cluster/EntityProxy#ConvertHttpApi`: Moved into core Effect and updated to the v4 HttpApiEndpoint and Schema types.

- `EntityProxy.ConvertRpcs` -> `effect/cluster/EntityProxy#ConvertRpcs`: Moved into core Effect and updated to the v4 Rpc and Schema type parameters.

## `@effect/cluster/EntityProxyServer`

- `EntityProxyServer.RpcHandlers` -> `effect/cluster/EntityProxyServer#RpcHandlers`: Moved into core Effect and updated for the additional v4 Rpc requirements type parameter.

- `EntityProxyServer.layerHttpApi` -> `effect/cluster/EntityProxyServer#layerHttpApi`: Moved into core Effect. Use v4 HttpApi identifiers and provide Sharding plus both Rpc.ServicesServer and Rpc.ServicesClient codec requirements.

- `EntityProxyServer.layerRpcHandlers` -> `effect/cluster/EntityProxyServer#layerRpcHandlers`: Moved into core Effect; replace Rpc.Context with both Rpc.ServicesServer and Rpc.ServicesClient codec requirements, alongside Sharding.

## `@effect/cluster/EntityResource`

- `EntityResource.TypeId` -> `effect/cluster/EntityResource#TypeId`: Moved into core Effect; its literal changed to \~effect/cluster/EntityResource.

- `EntityResource.make` -> `effect/cluster/EntityResource#make`: Moved into core Effect. Acquisition is lazy by default in v4; set acquireEagerly: true to preserve v3 behavior.

## `@effect/cluster/Envelope`

- `Envelope.Envelope.Encoded` -> `effect/cluster/Envelope#Encoded`: The encoded envelope union moved to the module-level Encoded type.

- `Envelope.Envelope.PartialEncoded` -> `effect/cluster/Envelope#Partial`: The partially decoded runtime union was renamed to Partial; use PartialJson for its JSON codec.

- `Envelope.EnvelopeFromSelf` -> `effect/cluster/Envelope#Envelope`: The self schema was renamed to Envelope and declaration-merges with the envelope type and namespace.

- `Envelope.PartialEncoded` -> `effect/cluster/Envelope#PartialJson`: The partially decoded envelope JSON codec was renamed to PartialJson.

- `Envelope.PartialEncodedArray` -> `effect/cluster/Envelope#PartialArray`: The mutable array codec was renamed to PartialArray.

- `Envelope.PartialEncodedFromSelf` -> `effect/cluster/Envelope#Partial`: The separate self schema was folded into Partial; derive JSON encoding with PartialJson.

- `Envelope.PartialEncodedRequest` -> `Schema.toCodecJson(Envelope.PartialRequest)`: V4 exports the self schema as PartialRequest and derives its JSON codec with Schema.toCodecJson.

- `Envelope.PartialEncodedRequestFromSelf` -> `effect/cluster/Envelope#PartialRequest`: The partially decoded request self schema was renamed to PartialRequest.

- `Envelope.Request` -> `effect/cluster/Envelope#Request`: The request interface remains and declaration-merges with the exported Request schema.

- `Envelope.Request.Encoded` -> `effect/cluster/Envelope#PartialRequestEncoded`: The JSON request shape moved to the module-level PartialRequestEncoded interface.

- `Envelope.Request.PartialEncoded` -> `effect/cluster/Envelope#PartialRequest`: The partially decoded request shape moved to the module-level PartialRequest class and type.

- `Envelope.RequestFromSelf` -> `effect/cluster/Envelope#Request`: The request self schema was renamed to Request and declaration-merges with the runtime interface.

- `Envelope.TypeId` -> `typeof Envelope.TypeId`: The marker value remains, but the type alias was removed and the value is now a string literal; use typeof in type position.

## `@effect/cluster/MachineId`

- `MachineId.make` -> `effect/cluster/MachineId#make`: Moved into core Effect. The v4 helper is an unchecked cast; validate external input with the MachineId schema when needed.

## `@effect/cluster/Message`

- `Message.serialize` -> `effect/cluster/Message#serialize`: Moved into core Effect. Pass the transport's codecFor as the second argument; use serializeEnvelope for the JSON Envelope.Encoded form.

## `@effect/cluster/MessageStorage`

- `MessageStorage.Encoded` -> `effect/cluster/MessageStorage#Encoded`: Moved into core Effect; use the v4 Envelope.Encoded and Reply.Encoded aliases. Custom drivers now implement batched resetAddresses, and unprocessedMessages receives optional limit and address filters.

- `MessageStorage.make` -> `effect/cluster/MessageStorage#make`: Moved into core Effect. Context service projections now use the Service property instead of Type. Custom service implementations must also provide resetAddresses for batched mailbox resets.

- `MessageStorage.makeEncoded` -> `effect/cluster/MessageStorage#makeEncoded`: Moved into core Effect. Custom encoded drivers must replace resetAddress with resetAddresses and may use the new limit and addresses options passed to unprocessedMessages.

## `@effect/cluster/Reply`

- `Reply.Reply` -> `Reply.Reply(rpc, codecFor)`: Pass the transport codec. Decoding replies requires Rpc.ServicesClient; encoding replies requires Rpc.ServicesServer.

- `Reply.ReplyEncoded` -> `effect/cluster/Reply#Encoded`: Renamed to Encoded and no longer parameterized by an Rpc; payload fields are unknown and validated by Reply.Reply(rpc, codecFor) with the transport's codec.

- `Reply.TypeId` -> `none`: The reply marker is private in v4. Use Reply.isReply for runtime refinement.

- `Reply.serialize` -> `effect/cluster/Reply#serialize`: Moved into core Effect and now returns the non-generic Reply.Encoded wire union. Pass the transport's codecFor as the second argument.

## `@effect/cluster/Runner`

- `Runner.TypeId` -> `none`: The runner marker is private in v4. Use the exported Runner class and schema.

## `@effect/cluster/RunnerAddress`

- `RunnerAddress.TypeId` -> `none`: The runner-address marker is private in v4. Use the exported RunnerAddress class and schema.

## `@effect/cluster/RunnerStorage`

- `RunnerStorage.makeMemory` -> `effect/cluster/RunnerStorage#makeMemory`: Moved into core Effect; it still constructs the in-memory RunnerStorage service implementation.

## `@effect/cluster/Runners`

- `Runners.make` -> `effect/cluster/Runners#make`: Moved into core Effect. Its options now require codecFor; pass the codec used by the remote runner transport, such as RpcSerialization.json.codecFor for JSON. Context service projections now use Service instead of Type.

- `Runners.makeNoop` -> `effect/cluster/Runners#makeNoop`: Moved into core Effect; it returns the Context.Service implementation through the Service projection instead of Type.

## `@effect/cluster/ShardId`

- `ShardId.ShardId` -> `effect/cluster/ShardId#ShardId`: The class became a merged interface and schema value. Use ShardId.make; former static parsers and printers are module functions.

- `ShardId.TypeId` -> `none`: The shard marker is private in v4. Use ShardId.isShardId for runtime refinement.

## `@effect/cluster/ShardingConfig`

- `ShardingConfig.config` -> `effect/cluster/ShardingConfig#config`: Moved into core Effect; its Context service value type now uses the Service property instead of Type.

- `ShardingConfig.defaults` -> `effect/cluster/ShardingConfig#defaults`: Moved into core Effect; service type projections now use Service instead of Type. V4 also defaults maxResidentEntities to 10,000 and unprocessedMessageBatchSize to 1,024.

- `ShardingConfig.layer` -> `effect/cluster/ShardingConfig#layer`: Moved into core Effect with the same shallow default merge; service type projections now use Service instead of Type.

## `@effect/cluster/ShardingRegistrationEvent`

- `ShardingRegistrationEvent.match` -> `effect/cluster/ShardingRegistrationEvent#match`: Moved into core Effect with the same tagged-enum matcher.

## `@effect/cluster/SingleRunner`

- `SingleRunner.layer` -> `effect/cluster/SingleRunner#layer`: Moved into core Effect. V4 additionally requires Crypto.Crypto because SQL message storage hashes long deduplication keys.

## `@effect/cluster/SingletonAddress`

- `SingletonAddress.TypeId` -> `none`: The singleton-address marker is private in v4. Use the exported SingletonAddress class and schema.

## `@effect/cluster/Snowflake`

- `Snowflake.Generator` -> `effect/cluster/Snowflake#Generator`: Moved into core Effect and changed to Context.Service; its unsafeNext method was renamed to nextUnsafe.

- `Snowflake.TypeId` -> `effect/cluster/Snowflake#TypeId`: Moved into core Effect; the public marker is now the string literal \~effect/cluster/Snowflake.

## `@effect/cluster/SqlMessageStorage`

- `SqlMessageStorage.layer` -> `effect/cluster/SqlMessageStorage#layer`: Moved into core Effect. V4 adds a Crypto.Crypto requirement for hashing long deduplication keys.

- `SqlMessageStorage.layerWith` -> `effect/cluster/SqlMessageStorage#layerWith`: Moved into core Effect with the same optional table prefix; v4 additionally requires Crypto.Crypto.

- `SqlMessageStorage.make` -> `effect/cluster/SqlMessageStorage#make`: Moved into core Effect with the same prefix option; v4 additionally requires Crypto.Crypto.

## `@effect/cluster/SqlRunnerStorage`

- `SqlRunnerStorage.make` -> `effect/cluster/SqlRunnerStorage#make`: Moved into core Effect with the same prefix option and service requirements.
