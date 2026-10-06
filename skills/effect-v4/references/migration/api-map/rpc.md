# v3 → v4 API map: @effect/rpc (10 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/rpc/Rpc`

- `Rpc.AddError` -> `effect/rpc/Rpc#AddError`: Retained; the added error must now be a Schema.Top and the resulting RPC also preserves its explicit service requirements.

- `Rpc.AddMiddleware` -> `effect/rpc/Rpc#AddMiddleware`: Retained; middleware is now an RpcMiddleware.AnyService and its provides/requires metadata updates the RPC service requirements.

- `Rpc.Any` -> `effect/rpc/Rpc#Any`: Retained as the erased RPC shape; use AnyWithProps when schema and middleware fields are required.

- `Rpc.AnySchema` -> `Schema.Top`: The RPC-specific erased schema alias was removed; use the v4 top schema constraint.

- `Rpc.AnyTaggedRequestSchema` -> `none`: RpcGroup no longer converts Schema.TaggedRequest classes into RPCs; declare the contract explicitly with Rpc.make.

- `Rpc.Context` -> `effect/rpc/Rpc#Services`: Schema Context became decoding and encoding services; use Services, or ServicesClient / ServicesServer at the corresponding boundary.

- `Rpc.ErrorEncoded` -> `Rpc.ErrorSchema<R>["Encoded"]`: The alias was removed; index the v4 error schema's Encoded member directly.

- `Rpc.ErrorExitEncoded` -> `Rpc.ErrorExitSchema<R>["Encoded"]`: Use the new exit error schema, which includes stream and middleware errors, then select its Encoded member.

- `Rpc.ErrorSchema` -> `effect/rpc/Rpc#ErrorSchema`: Retained; middleware errors now come from each service's error metadata.

- `Rpc.Handler` -> `effect/rpc/Rpc#Handler`: Retained; handler metadata now supplies ServerClient, RequestId, headers, and the concrete RPC.

- `Rpc.Middleware` -> `effect/rpc/Rpc#Middleware`: Retained and extracts Context.Service identifiers from the attached middleware services.

- `Rpc.MiddlewareClient` -> `effect/rpc/Rpc#MiddlewareClient`: Retained; required client middleware is derived from services configured with requiredForClient.

- `Rpc.Payload` -> `effect/rpc/Rpc#Payload`: Retained as the decoded payload type; use PayloadConstructor for the input accepted by generated clients.

- `Rpc.Success` -> `effect/rpc/Rpc#Success`: Retained as the decoded success type.

- `Rpc.SuccessChunkEncoded` -> `Rpc.SuccessExitSchema<R>["Encoded"]`: The alias was removed; for a streaming RPC the exit success schema is the stream element schema.

- `Rpc.SuccessEncoded` -> `effect/rpc/Rpc#SuccessEncoded`: Retained after the module move and now accounts for the RPC's explicit service-requirement parameter.

- `Rpc.SuccessExitEncoded` -> `Rpc.SuccessExitSchema<R>["Encoded"]`: Use the new exit success schema and select its Encoded member; streaming RPC exits use the element schema separately from the terminal void exit.

- `Rpc.SuccessSchema` -> `effect/rpc/Rpc#SuccessSchema`: Retained and uses the v4 Schema.Top constraint.

- `Rpc.Tag` -> `effect/rpc/Rpc#Tag`: Retained and also accounts for the v4 RPC service-requirement parameter.

- `Rpc.TypeId` -> `none`: The RPC marker is private in v4; use Rpc.isRpc for runtime checks and Rpc.Any for type constraints.

- `Rpc.WrapperTypeId` -> `none`: The wrapper marker is private in v4; use Rpc.isWrapper and the public Wrapper type.

- `Rpc.fromTaggedRequest` -> `Rpc.make`: Automatic TaggedRequest conversion was removed; pass the tag, payload, success, and error schemas explicitly to Rpc.make.

- `Rpc.make` -> `effect/rpc/Rpc#make`: Retained; schemas use v4 Schema.Top constraints and the defect option accepts Rpc.DefectSchema.

- `Rpc.wrap` -> `effect/rpc/Rpc#wrap`: Retained after the module move; it still applies fork and uninterruptible handler options, while the return type is now uniformly Rpc.Wrapper.

## `@effect/rpc/RpcClient`

- `RpcClient.Protocol` -> `effect/rpc/RpcClient#Protocol`: Retained as a Context.Service; custom transports now route multiple client ids through run and send.

- `RpcClient.RpcClient.From` -> `effect/rpc/RpcClient#RpcClient.From`: Generated clients now preserve full RPC tags as property names, remove the Prefix type parameter, and expose streaming results through the asQueue option instead of asMailbox.

- `RpcClient.RpcClient.NonPrefixed` -> `none`: The prefix-partition helper was removed; v4 clients map every RPC tag directly to an object property.

- `RpcClient.RpcClient.Prefixes` -> `none`: Nested prefix client objects were removed; v4 preserves the full RPC tag as the generated client property.

- `RpcClient.currentHeaders` -> `effect/rpc/RpcClient#CurrentHeaders`: Renamed and changed from FiberRef to Context.Reference; prefer RpcClient.withHeaders for scoped overrides.

- `RpcClient.makeProtocolHttp` -> `effect/rpc/RpcClient#makeProtocolHttp`: Retained; it creates the Protocol service implementation from an HttpClient.

- `RpcClient.withHeadersEffect` -> `Effect.flatMap(headers, (value) => RpcClient.withHeaders(effect, value))`: withHeaders now accepts Headers.Input synchronously; evaluate effectful headers first and then scope the client effect.

## `@effect/rpc/RpcClientError`

- `RpcClientError.TypeId` -> `none`: The marker is private in v4; narrow with instanceof RpcClientError or inspect the public \_tag.

## `@effect/rpc/RpcGroup`

- `RpcGroup.Any` -> `effect/rpc/RpcGroup#Any`: Moved unchanged as the erased RpcGroup constraint.

- `RpcGroup.HandlerContext` -> `effect/rpc/RpcGroup#HandlerServices`: Renamed for v4 service terminology and now includes explicit RPC requirements after removing middleware-provided services.

- `RpcGroup.HandlersContext` -> `effect/rpc/RpcGroup#HandlersServices`: Renamed; it unions HandlerServices across the handler object.

- `RpcGroup.TypeId` -> `none`: The group marker is private in v4; use RpcGroup.Any for an erased group constraint.

## `@effect/rpc/RpcMessage`

- `RpcMessage.FromServerEncoded` -> `effect/rpc/RpcMessage#FromServerEncoded`: The union is retained and now also includes RequestEncoded for server-originated requests and notifications. Handle \_tag: "Request" when matching exhaustively; isNotification identifies notifications.

- `RpcMessage.RequestId` -> `effect/rpc/RpcMessage#RequestId`: Request ids are now branded string or number values; convert bigint ids before calling the retained RequestId constructor.

- `RpcMessage.RequestIdTypeId` -> `effect/rpc/RpcMessage#RequestId`: The public symbol marker was removed; use the branded RequestId type and RequestId constructor rather than inspecting its brand.

## `@effect/rpc/RpcMiddleware`

- `RpcMiddleware.RpcMiddlewareWrap` -> `effect/rpc/RpcMiddleware#RpcMiddleware`: The wrap and non-wrap shapes were unified; implement a function receiving the handler effect and request options.

- `RpcMiddleware.Tag` -> `effect/rpc/RpcMiddleware#Service`: Renamed and redesigned with explicit requires, provides, clientError, error, and requiredForClient configuration.

- `RpcMiddleware.TagClass` -> `effect/rpc/RpcMiddleware#ServiceClass`: Renamed class type for the v4 Context.Service-based middleware declaration.

- `RpcMiddleware.TagClass.Failure` -> `effect/rpc/RpcMiddleware#Error`: Failure terminology became error; apply the extractor to the middleware ID.

- `RpcMiddleware.TagClass.FailureContext` -> `effect/rpc/RpcMiddleware#ErrorServicesEncode / ErrorServicesDecode`: The single schema context split into server encoding and client decoding services.

- `RpcMiddleware.TagClass.FailureSchema` -> `effect/rpc/RpcMiddleware#ErrorSchema`: Renamed and applied to the middleware ID rather than constructor options.

- `RpcMiddleware.TagClass.FailureService` -> `effect/rpc/RpcMiddleware#Error`: Use the decoded error extractor; optional middleware fallback was removed.

- `RpcMiddleware.TagClass.Optional` -> `none`: Optional declaration and fallback-on-failure behavior were removed; model fallback inside the middleware effect.

- `RpcMiddleware.TagClass.Provides` -> `effect/rpc/RpcMiddleware#Provides`: Moved to the module level and applied to the middleware ID metadata.

- `RpcMiddleware.TagClass.RequiredForClient` -> `RpcMiddleware.ServiceClass["requiredForClient"]`: The standalone options extractor was removed; the boolean is exposed directly by the resulting service class.

- `RpcMiddleware.TagClassAny` -> `effect/rpc/RpcMiddleware#AnyService`: Renamed widened middleware service-key shape.

- `RpcMiddleware.TagClassAnyWithProps` -> `effect/rpc/RpcMiddleware#AnyServiceWithProps`: Renamed erased service key whose value has the unified server middleware function shape.

- `RpcMiddleware.TypeId` -> `effect/rpc/RpcMiddleware#TypeId`: Retained as the public middleware metadata marker and now has a string-literal type.

- `RpcMiddleware.layerClient` -> `effect/rpc/RpcMiddleware#layerClient`: Retained; the client function can now modify the typed Request passed to next and carry a client-only error type.

## `@effect/rpc/RpcSchema`

- `RpcSchema.Stream` -> `effect/rpc/RpcSchema#Stream`: Retained as both the stream schema interface and constructor; error is the second argument and schema services are split by direction.

- `RpcSchema.StreamSchemaId` -> `none`: The stream marker is private in v4; use RpcSchema.isStreamSchema and getStreamSchemas.

- `RpcSchema.getStreamSchemas` -> `effect/rpc/RpcSchema#getStreamSchemas`: Retained for internal-style schema inspection; pass the schema itself rather than its AST.

- `RpcSchema.isStreamSchema` -> `effect/rpc/RpcSchema#isStreamSchema`: Retained; it accepts a v4 Schema.Constraint.

- `RpcSchema.isStreamSerializable` -> `RpcSchema.isStreamSchema(schema)`: The separate WithResult serializability predicate was removed; v4 RPC streaming is identified by its explicit Stream schema.

## `@effect/rpc/RpcSerialization`

- `RpcSerialization.RpcSerializationError` -> `effect/rpc/RpcSerialization#MaxBufferSizeExceeded`: Buffer-limit failures now use MaxBufferSizeExceeded. MessagePack-specific decode errors have no counterpart.

- `RpcSerialization.layerMsgPack` -> `effect/rpc/RpcSerialization#layerSchemaBinary`: MessagePack RPC serialization was removed. Use SchemaBinary, or layerNdjson when you need newline-delimited JSON framing.

- `RpcSerialization.layerMsgPackWith` -> `effect/rpc/RpcSerialization#layerSchemaBinary`: MessagePack RPC serialization was removed. Pass maxFrameSize to layerSchemaBinary; NDJSON buffer limits remain on layerNdjsonWith.

- `RpcSerialization.makeMsgPack` -> `effect/rpc/RpcSerialization#layerSchemaBinary`: MessagePack RPC serialization was removed. Construct SchemaBinary serialization with layerSchemaBinary.

- `RpcSerialization.msgPack` -> `effect/rpc/RpcSerialization#layerSchemaBinary`: The MessagePack RpcSerialization service value was removed. Provide layerSchemaBinary instead.

## `@effect/rpc/RpcServer`

- `RpcServer.Protocol` -> `effect/rpc/RpcServer#Protocol`: Retained as a Context.Service; custom transports now expose a disconnect queue, explicit capability flags, and codecFor for schema-aware payload and exit encoding.

- `RpcServer.fiberIdClientInterrupt` -> `effect/rpc/RpcSchema#ClientAbort`: The sentinel FiberId was replaced by a Cause annotation; inspect ClientAbort in the interruption cause when client cancellation must be distinguished.

- `RpcServer.fiberIdTransientInterrupt` -> `none`: The internal transient sentinel was removed; protocol shutdown and disconnect now interrupt with the active parent fiber identity.

- `RpcServer.layer` -> `effect/rpc/RpcServer#layer`: Moved to core Effect; server requirements are now derived with Rpc.ServicesServer rather than the former combined Rpc.Context alias.

- `RpcServer.layerHttpRouter` -> `effect/rpc/RpcServer#layerHttp`: Renamed; it installs an HTTP or WebSocket RPC route into the v4 HttpRouter service.

- `RpcServer.layerProtocolHttp` -> `effect/rpc/RpcServer#layerProtocolHttp`: Retained; v4 has one HttpRouter service and no router tag option.

- `RpcServer.layerProtocolHttpRouter` -> `effect/rpc/RpcServer#layerProtocolHttp`: The separate layer-router variant was unified with layerProtocolHttp.

- `RpcServer.layerProtocolWebsocketRouter` -> `effect/rpc/RpcServer#layerProtocolWebsocket`: Renamed after the HTTP router services were unified.

- `RpcServer.make` -> `effect/rpc/RpcServer#make`: Retained; schema encoding services are now explicit server requirements.

- `RpcServer.makeProtocolHttp` -> `effect/rpc/RpcServer#makeProtocolHttp`: Retained; it registers a POST route in the current v4 HttpRouter.

- `RpcServer.makeProtocolHttpRouter` -> `effect/rpc/RpcServer#makeProtocolHttp`: The separate router constructor was unified with makeProtocolHttp.

- `RpcServer.makeProtocolWebsocketRouter` -> `effect/rpc/RpcServer#makeProtocolWebsocket`: Renamed after the HTTP router services were unified.

- `RpcServer.makeProtocolWithHttpApp` -> `effect/rpc/RpcServer#makeProtocolWithHttpEffect`: HttpApp became HttpEffect; the result contains protocol and httpEffect.

- `RpcServer.makeProtocolWithHttpAppWebsocket` -> `effect/rpc/RpcServer#makeProtocolWithHttpEffectWebsocket`: HttpApp became HttpEffect; the result contains the WebSocket protocol and upgrade effect.

- `RpcServer.toHttpApp` -> `effect/rpc/RpcServer#toHttpEffect`: Renamed for the v4 HTTP effect model; it starts the RPC server and returns the request effect.

- `RpcServer.toHttpAppWebsocket` -> `effect/rpc/RpcServer#toHttpEffectWebsocket`: Renamed for the v4 HTTP effect model; it returns the WebSocket upgrade effect.

- `RpcServer.toWebHandler` -> `HttpRouter.toWebHandler(RpcServer.layerHttp(options).pipe(Layer.provide(options.layer)))`: The RPC convenience wrapper was removed; build the RPC route layer and convert it with the generic v4 HttpRouter web-handler adapter.

## `@effect/rpc/RpcTest`

- `RpcTest.makeClient` -> `effect/rpc/RpcTest#makeClient`: Retained; it uses the v4 no-serialization client/server path and requires handlers plus any server and client middleware services.
