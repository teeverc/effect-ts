# v3 → v4 API map: @effect/platform (55 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/platform/ChannelSchema`

- `ChannelSchema.decode` -> `ChannelSchema.decode`: The API moved to effect/ChannelSchema; update the import and adapt the schema to the v4 Schema.Constraint model.

- `ChannelSchema.duplex` -> `ChannelSchema.duplex`: The API moved to effect/ChannelSchema; update the import and adapt the schema to the v4 Schema.Constraint model.

- `ChannelSchema.encode` -> `ChannelSchema.encode`: The API moved to effect/ChannelSchema; update the import and adapt the schema to the v4 Schema.Constraint model.

## `@effect/platform/Command`

- `Command.Command` -> `ChildProcess.Command`: Commands moved to effect/process/ChildProcess and are now directly Effectable to spawn a ChildProcessHandle.

- `Command.Command.Input` -> `ChildProcess.CommandInput`: The standard-input configuration type was flattened out of the Command namespace.

- `Command.Command.Proto` -> `ChildProcess.StandardCommand | ChildProcess.PipedCommand`: The public command prototype was removed; narrow the Command union to its StandardCommand or PipedCommand interfaces.

- `Command.CommandTypeId` -> `none`: The command type-id alias is internal in v4; use ChildProcess.Command or ChildProcess.isCommand instead.

- `Command.env` -> `ChildProcess.setEnv`: Use the renamed command combinator.

- `Command.exitCode` -> `ChildProcessSpawner.ChildProcessSpawner.exitCode`: Obtain the ChildProcessSpawner service and call exitCode, or spawn the Effectable command and read the handle exitCode.

- `Command.feed` -> `ChildProcess.CommandOptions["stdin"]`: The feed combinator was removed; pass a Stream as stdin when constructing the command.

- `Command.flatten` -> `none`: No flatten helper remains; inspect StandardCommand and PipedCommand recursively when command structure is required.

- `Command.lines` -> `ChildProcessSpawner.ChildProcessSpawner.lines`: Output collection moved onto the ChildProcessSpawner service.

- `Command.runInShell` -> `ChildProcess.CommandOptions["shell"]`: Set shell when calling ChildProcess.make; there is no post-construction shell combinator.

- `Command.start` -> `ChildProcessSpawner.ChildProcessSpawner.spawn`: Use the spawner service, or yield the Effectable ChildProcess.Command directly, to obtain a ChildProcessHandle.

- `Command.stderr` -> `ChildProcess.CommandOptions["stderr"]`: Configure stderr in ChildProcess.make options; the standalone combinator was removed.

- `Command.stdin` -> `ChildProcess.CommandOptions["stdin"]`: Configure stdin in ChildProcess.make options; the standalone combinator was removed.

- `Command.stdout` -> `ChildProcess.CommandOptions["stdout"]`: Configure stdout in ChildProcess.make options; the standalone combinator was removed.

- `Command.stream` -> `ChildProcessSpawner.ChildProcessSpawner.spawn + ChildProcessHandle.stdout`: Spawn within a scope and consume the returned handle's stdout stream.

- `Command.streamLines` -> `ChildProcessSpawner.ChildProcessSpawner.streamLines`: Text-line streaming moved onto the ChildProcessSpawner service.

- `Command.string` -> `ChildProcessSpawner.ChildProcessSpawner.string`: Output collection moved onto the ChildProcessSpawner service.

- `Command.workingDirectory` -> `ChildProcess.setCwd`: Use the renamed command combinator.

## `@effect/platform/CommandExecutor`

- `CommandExecutor.CommandExecutor` -> `ChildProcessSpawner.ChildProcessSpawner`: The executor service moved to effect/process/ChildProcessSpawner and was renamed.

- `CommandExecutor.Process` -> `ChildProcessSpawner.ChildProcessHandle`: Running-process handles were renamed and moved to ChildProcessSpawner.

- `CommandExecutor.Process.Id` -> `ChildProcessSpawner.ProcessId`: The process-id brand is now exported directly.

- `CommandExecutor.ProcessTypeId` -> `none`: The ChildProcessHandle marker is internal in v4; use the ChildProcessHandle interface.

- `CommandExecutor.TypeId` -> `none`: The Context.Service class replaces the public executor type-id alias.

- `CommandExecutor.makeExecutor` -> `ChildProcessSpawner.make`: Use the renamed constructor; it derives output helpers from a spawn implementation.

## `@effect/platform/Cookies`

- `Cookies.CookieTypeId` -> `Cookies.isCookie`: The cookie brand is private in v4; use the public refinement instead of reading the type-id symbol.

- `Cookies.CookiesError` -> `Cookies.CookiesError`: The error tag is CookiesError rather than CookieError. Update catchTag calls and \_tag comparisons; validation details are in the reason field.

- `Cookies.ErrorTypeId` -> `Cookies.CookiesError`: The error brand is private in v4; identify the exported error class instead.

- `Cookies.TypeId` -> `Cookies.isCookies`: The collection brand is private in v4; use the public refinement instead.

- `Cookies.unsafeMakeCookie` -> `Cookies.makeCookieUnsafe`: Renamed to put Unsafe last; it still throws on invalid cookie data.

- `Cookies.unsafeSet` -> `Cookies.setUnsafe`: Renamed to put Unsafe last; the dual throwing behavior is retained.

- `Cookies.unsafeSetAll` -> `Cookies.setAllUnsafe`: Renamed to put Unsafe last; the dual all-or-throw behavior is retained.

## `@effect/platform/Error`

- `Error.Module` -> `string`: The closed module-name Schema was removed; PlatformError reason records accept any module string.

- `Error.PlatformError` -> `PlatformError.PlatformError`: The module moved to effect/PlatformError and PlatformError became a wrapper class around BadArgument or SystemError.

- `Error.SystemErrorReason` -> `PlatformError.SystemErrorTag`: The normalized system-error reason union was renamed.

- `Error.TypeId` -> `none`: The PlatformError runtime marker is internal in v4; use the PlatformError class/tag.

- `Error.TypeIdError` -> `Data.TaggedError or Schema.Error`: The platform-specific error-class factory was removed; define tagged data errors or schema-backed error classes directly.

## `@effect/platform/Etag`

- `Etag.GeneratorTypeId` -> `Etag.Generator`: The standalone generator brand was removed; Generator is now a Context.Service class.

- `Etag.layer` -> `Etag.layer`: Retained; it still provides the strong metadata-based ETag Generator service.

## `@effect/platform/FetchHttpClient`

- `FetchHttpClient.Fetch` -> `FetchHttpClient.Fetch`: Retained as a Context.Reference that defaults to globalThis.fetch.

- `FetchHttpClient.layer` -> `FetchHttpClient.layer`: Retained as the HttpClient layer backed by the configured Fetch reference.

## `@effect/platform/FileSystem`

- `FileSystem.AccessFileOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["access"]>[1]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.CopyOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["copy"]>[2]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.File.Descriptor` -> `none`: Native file descriptors are no longer part of the portable File interface; use the File methods and keep any platform handle private in custom implementations.

- `FileSystem.FileDescriptor` -> `none`: The descriptor branding constructor was removed with the public fd field; use File operations instead of exposing a native descriptor.

- `FileSystem.FileTypeId` -> `typeof FileSystem.FileTypeId`: The runtime marker remains exported, but the separate type alias was removed.

- `FileSystem.GiB` -> `ByteSize.gibibytes`: Use the ByteSize binary unit constructor.

- `FileSystem.KiB` -> `ByteSize.kibibytes`: Use the ByteSize binary unit constructor.

- `FileSystem.MakeDirectoryOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["makeDirectory"]>[1]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.MakeTempDirectoryOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["makeTempDirectory"]>[0]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.MakeTempFileOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["makeTempFile"]>[0]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.MiB` -> `ByteSize.mebibytes`: Use the ByteSize binary unit constructor.

- `FileSystem.OpenFileOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["open"]>[1]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.PiB` -> `ByteSize.pebibytes`: Use the ByteSize binary unit constructor.

- `FileSystem.ReadDirectoryOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["readDirectory"]>[1]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.RemoveOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["remove"]>[1]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.SinkOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["sink"]>[1]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.Size` -> `ByteSize.ByteSize`: Use ByteSize.bytes or unit constructors for file sizes. Truncation lengths, buffer sizes, and read/write counts use number. File.seek takes and returns signed bigint positions; it can fail with PlatformError, including BadArgument when seeking before the start.

- `FileSystem.SizeInput` -> `ByteSize.Input`: File-size and path-backed range inputs use ByteSize.Input. String inputs must be non-negative decimal integers without leading zeros, followed by a canonical unit symbol or lowercase unit name, with at most one separating space. Parse external strings or fractional quantities with ByteSize.fromString (Option) or ByteSize.fromStringUnsafe (throws), then pass the resulting ByteSize. Truncation lengths, Web File ranges, and buffer sizes use number.

- `FileSystem.StreamOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["stream"]>[1]>`: Stream options are inline; bufferSize was removed, bytesToRead and offset accept ByteSize inputs, and chunkSize uses number.

- `FileSystem.TiB` -> `ByteSize.tebibytes`: Use the ByteSize binary unit constructor.

- `FileSystem.WatchEventCreate` -> `FileSystem.WatchEvent.Create`: The constructor was removed; construct a tagged object with \_tag: "Create" and path.

- `FileSystem.WatchEventRemove` -> `FileSystem.WatchEvent.Remove`: The constructor was removed; construct a tagged object with \_tag: "Remove" and path.

- `FileSystem.WatchEventUpdate` -> `FileSystem.WatchEvent.Update`: The constructor was removed; construct a tagged object with \_tag: "Update" and path.

- `FileSystem.WatchOptions` -> `FileSystem.WatchOptions`: Retained after moving the module to effect/FileSystem; pass `{ recursive: true }` as the optional second argument to FileSystem.watch.

- `FileSystem.WriteFileOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["writeFile"]>[2]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.WriteFileStringOptions` -> `NonNullable<Parameters<FileSystem.FileSystem["writeFileString"]>[2]>`: Operation option interfaces are inline in the v4 FileSystem service.

- `FileSystem.layerNoop` -> `FileSystem.layerNoop`: The helper remains after moving the module to effect/FileSystem.

- `FileSystem.make` -> `FileSystem.make`: The constructor remains after moving the module to effect/FileSystem; adapt the implementation to the v4 service shape.

## `@effect/platform/Headers`

- `Headers.Headers` -> `Headers.Headers`: Import Headers from effect/http; the immutable string-record interface is retained with its v4 TypeId brand.

- `Headers.HeadersTypeId` -> `Headers.TypeId`: The public Headers type-id symbol was renamed from HeadersTypeId to TypeId.

- `Headers.currentRedactedNames` -> `Headers.CurrentRedactedNames`: Renamed and changed from FiberRef to Context.Reference; override it with service provisioning.

- `Headers.remove` -> `Headers.remove / Headers.removeMany`: Use remove for one name or removeMany for an iterable; RegExp removal requires enumerating matching names.

- `Headers.schema` -> `Schema.Headers`: The encoded-record and self schemas were consolidated and moved to effect/Schema as Schema.Headers.

- `Headers.schemaFromSelf` -> `Schema.Headers`: The encoded-record and self schemas were consolidated and moved to effect/Schema as Schema.Headers.

- `Headers.unsafeFromRecord` -> `Headers.fromRecordUnsafe`: Renamed to put Unsafe last; it still skips name normalization.

## `@effect/platform/HttpApi`

- `HttpApi.Api` -> `none`: The Context tag carrying the API was removed. Pass the HttpApi value explicitly to builders and clients.

- `HttpApi.HttpApi.Any` -> `effect/http-api/HttpApi#Constraint`: Use the erased marker constraint when only HttpApi identity is needed.

- `HttpApi.HttpApi.AnyWithProps` -> `effect/http-api/HttpApi#Top`: Use the widened HttpApi type that retains runtime properties.

- `HttpApi.TypeId` -> `none`: The marker is private in v4; use HttpApi.isHttpApi for runtime narrowing and Constraint or Top for types.

- `HttpApi.make` -> `effect/http-api/HttpApi#make`: The constructor remains, but API-wide error and service parameters were removed; declare errors on endpoints and attach middleware.

## `@effect/platform/HttpApiBuilder`

- `HttpApiBuilder.Handlers` -> `effect/http-api/HttpApiBuilder#Handlers`: Handlers now tracks an endpoint map and handled identifiers. Prefer Handlers.FromGroup\<Group\>.

- `HttpApiBuilder.Handlers.Error` -> `effect/http-api/HttpApiBuilder#Handlers.Error`: The helper remains and extracts the error channel of an effectful group-builder return.

- `HttpApiBuilder.Handlers.Middleware` -> `none`: The handler-internal HttpApp middleware alias was removed. Use HttpRouter.middleware inference or HttpRouter.middleware.Fn.

- `HttpApiBuilder.Handlers.ValidateReturn` -> `effect/http-api/HttpApiBuilder#Handlers.ValidateReturn`: The validator remains and now checks the endpoint map against handled identifiers.

- `HttpApiBuilder.HandlersTypeId` -> `none`: The exported symbol was removed; do not inspect or construct the private Handlers marker.

- `HttpApiBuilder.Middleware` -> `none`: The API-specific middleware service tag was removed. Declared HttpApiMiddleware services are applied while routes are built; use HttpRouter.middleware for additional global middleware.

- `HttpApiBuilder.MiddlewareFn` -> `effect/http/HttpRouter#middleware.Fn`: HTTP apps are Effects in v4; use the router middleware function type or infer it through HttpRouter.middleware.

- `HttpApiBuilder.Router` -> `effect/http/HttpRouter#HttpRouter`: The API-specific router tag was removed; API and group layers register with the shared HttpRouter service.

- `HttpApiBuilder.api` -> `effect/http-api/HttpApiBuilder#layer`: Use layer(api) and provide the group layers; it registers the completed API with HttpRouter.

- `HttpApiBuilder.buildMiddleware` -> `none`: API-wide middleware assembly was removed. Declared HttpApiMiddleware services are applied while routes are built; use HttpRouter.middleware for additional middleware.

- `HttpApiBuilder.group` -> `effect/http-api/HttpApiBuilder#group`: The group layer remains; names are now identifiers and API/group global error channels are gone.

- `HttpApiBuilder.handler` -> `effect/http-api/HttpApiBuilder#handler`: The typed callback helper remains; names are now identifiers and API/group global error channels are gone. Pass the returned callback to handlers.handle.

- `HttpApiBuilder.httpApp` -> `effect/http/HttpRouter#toHttpEffect`: Build the application from the assembled API route layer; HTTP apps are Effects in v4.

- `HttpApiBuilder.middleware` -> `effect/http/HttpRouter#middleware`: Use router effect middleware and provide its layer to the API route layer; global middleware can target all router routes.

- `HttpApiBuilder.middlewareCors` -> `effect/http/HttpRouter#cors`: Use the router CORS layer, or provide route-scoped HttpMiddleware.cors through HttpRouter.middleware.

- `HttpApiBuilder.middlewareOpenApi` -> `effect/http-api/HttpApiBuilder#layer`: Set openapiPath in layer(api, options). The additionalPropertiesStrategy option was removed.

- `HttpApiBuilder.toWebHandler` -> `effect/http/HttpRouter#toWebHandler`: Pass the assembled API route layer to HttpRouter.toWebHandler; the handler and dispose lifecycle is retained.

## `@effect/platform/HttpApiClient`

- `HttpApiClient.Client.Method` -> `effect/http-api/HttpApiClient#Client.Method`: The type remains without GroupError. Requests use params/query and responseMode instead of path/urlParams and withResponse.

- `HttpApiClient.endpoint` -> `effect/http-api/HttpApiClient#endpoint`: The endpoint client remains, selected by group and endpoint identifiers and using v4 request and responseMode fields.

- `HttpApiClient.make` -> `effect/http-api/HttpApiClient#make`: The generated client remains; errors and services are now derived per endpoint and middleware.

- `HttpApiClient.makeWith` -> `effect/http-api/HttpApiClient#makeWith`: The supplied-HttpClient constructor remains and now requires endpoint client-middleware services.

## `@effect/platform/HttpApiEndpoint`

- `HttpApiEndpoint.HttpApiEndpoint` -> `effect/http-api/HttpApiEndpoint#HttpApiEndpoint`: The model remains, but its generics now carry path literals, schemas, middleware, and middleware services.

- `HttpApiEndpoint.HttpApiEndpoint.AddContext` -> `effect/http-api/HttpApiEndpoint#AddMiddleware`: Use AddMiddleware to add a middleware identifier and compute its service transformation.

- `HttpApiEndpoint.HttpApiEndpoint.AddError` -> `none`: Declare error schemas in the endpoint constructor options; the type helper and fluent addError method were removed.

- `HttpApiEndpoint.HttpApiEndpoint.Constructor` -> `none`: The tagged-template constructor type was removed; use HttpApiEndpoint.make(method)(identifier, path, options?).

- `HttpApiEndpoint.HttpApiEndpoint.Context` -> `effect/http-api/HttpApiEndpoint#ServerServices`: Use ServerServices for handler requirements; middleware IDs and extra requirements have separate extractors.

- `HttpApiEndpoint.HttpApiEndpoint.ContextWithName` -> `effect/http-api/HttpApiEndpoint#ServerServicesWithIdentifier`: Name became Identifier; combine with middleware extractors when the complete handler requirement union is needed.

- `HttpApiEndpoint.HttpApiEndpoint.Error` -> `effect/http-api/HttpApiEndpoint#Errors`: Use Errors for the decoded endpoint and middleware error union; v4 Error extracts the schema.

- `HttpApiEndpoint.HttpApiEndpoint.ErrorContext` -> `effect/http-api/HttpApiEndpoint#ErrorServicesEncode / ErrorServicesDecode`: The single schema context split into server encoding and client decoding services.

- `HttpApiEndpoint.HttpApiEndpoint.ErrorContextWithName` -> `effect/http-api/HttpApiEndpoint#ErrorServicesEncode / ErrorServicesDecode`: Select the endpoint with WithIdentifier, then apply the encode or decode service extractor.

- `HttpApiEndpoint.HttpApiEndpoint.ErrorWithName` -> `effect/http-api/HttpApiEndpoint#ErrorsWithIdentifier`: Renamed for identifier and returns the decoded endpoint plus middleware error union.

- `HttpApiEndpoint.HttpApiEndpoint.ExcludeName` -> `effect/http-api/HttpApiEndpoint#ExcludeIdentifier`: Direct rename from name to identifier.

- `HttpApiEndpoint.HttpApiEndpoint.ExtractPath` -> `none`: Tagged-template path extraction was removed. Put a params schema or field record in constructor option params.

- `HttpApiEndpoint.HttpApiEndpoint.HandlerRawWithName` -> `effect/http-api/HttpApiEndpoint#HandlerRawWithIdentifier`: Direct rename; raw request fields are now params and query.

- `HttpApiEndpoint.HttpApiEndpoint.HandlerWithName` -> `effect/http-api/HttpApiEndpoint#HandlerWithIdentifier`: Direct rename from name to identifier.

- `HttpApiEndpoint.HttpApiEndpoint.OptionalTypePropertySignature` -> `none`: Removed with the tagged-template path implementation.

- `HttpApiEndpoint.HttpApiEndpoint.PathEntries` -> `none`: Removed with tagged-template path extraction; declare endpoint params explicitly.

- `HttpApiEndpoint.HttpApiEndpoint.PathParsed` -> `effect/http-api/HttpApiEndpoint#Params`: Path data became params; Params extracts the schema, so use Params\<Endpoint\>["Type"] for decoded data.

- `HttpApiEndpoint.HttpApiEndpoint.Payload` -> `effect/http-api/HttpApiEndpoint#Payload`: The name remains but now extracts the schema; use Payload\<Endpoint\>["Type"] for buffered decoded data.

- `HttpApiEndpoint.HttpApiEndpoint.Success` -> `effect/http-api/HttpApiEndpoint#SuccessWithIdentifier`: Use SuccessWithIdentifier for the decoded, stream-aware result; v4 Success extracts the schema.

- `HttpApiEndpoint.HttpApiEndpoint.SuccessWithName` -> `effect/http-api/HttpApiEndpoint#SuccessWithIdentifier`: Direct rename from name to identifier; the result remains decoded and stream-aware.

- `HttpApiEndpoint.HttpApiEndpoint.UrlParams` -> `effect/http-api/HttpApiEndpoint#Query`: urlParams became query; Query extracts the schema, so use Query\<Endpoint\>["Type"] for decoded data.

- `HttpApiEndpoint.HttpApiEndpoint.ValidateHeaders` -> `effect/http-api/HttpApiEndpoint#HeadersConstraint`: Validation moved from an intersection helper to a constructor generic constraint.

- `HttpApiEndpoint.HttpApiEndpoint.ValidateParams` -> `none`: Tagged-template interpolation validation was removed; params are declared explicitly in options.params.

- `HttpApiEndpoint.HttpApiEndpoint.ValidatePath` -> `effect/http-api/HttpApiEndpoint#ParamsConstraint`: path became params and validation is now a constructor constraint.

- `HttpApiEndpoint.HttpApiEndpoint.ValidatePayload` -> `effect/http-api/HttpApiEndpoint#PayloadConstraint`: Payload validation is now a method-sensitive constructor constraint.

- `HttpApiEndpoint.HttpApiEndpoint.ValidateUrlParams` -> `effect/http-api/HttpApiEndpoint#QueryConstraint`: urlParams became query and validation is now a constructor constraint.

- `HttpApiEndpoint.PathSegment` -> `effect/http/HttpRouter#PathInput`: Path input moved to the shared router and is generalized to slash-prefixed paths or wildcard.

- `HttpApiEndpoint.TypeId` -> `none`: The endpoint type ID is private; use HttpApiEndpoint.isHttpApiEndpoint for runtime narrowing.

- `HttpApiEndpoint.get` -> `effect/http-api/HttpApiEndpoint#get`: Use get(identifier, path, options?); tagged templates and fluent schema setters were removed.

- `HttpApiEndpoint.head` -> `effect/http-api/HttpApiEndpoint#head`: Use head(identifier, path, options?); tagged templates and fluent schema setters were removed.

- `HttpApiEndpoint.make` -> `effect/http-api/HttpApiEndpoint#make`: The factory remains but now requires identifier, path, and options and applies codecs unless disabled.

- `HttpApiEndpoint.options` -> `effect/http-api/HttpApiEndpoint#options`: Same HTTP method constructor with the new identifier, path, and options signature.

- `HttpApiEndpoint.patch` -> `effect/http-api/HttpApiEndpoint#patch`: Same HTTP method constructor with the new identifier, path, and options signature.

- `HttpApiEndpoint.post` -> `effect/http-api/HttpApiEndpoint#post`: Same HTTP method constructor with the new identifier, path, and options signature.

- `HttpApiEndpoint.put` -> `effect/http-api/HttpApiEndpoint#put`: Same HTTP method constructor with the new identifier, path, and options signature.

## `@effect/platform/HttpApiError`

- `HttpApiError.Forbidden` -> `effect/http-api/HttpApiError#ForbiddenNoContent`: Use ForbiddenNoContent to preserve the empty 403 wire schema; Forbidden now has a JSON-tagged body.

- `HttpApiError.HttpApiDecodeError` -> `effect/http-api/HttpApiError#HttpApiSchemaError`: Validation now stores kind and a SchemaError cause and is a defect unless transformed by schema-error middleware.

- `HttpApiError.Issue` -> `effect/SchemaIssue#Issue`: Structured failures now live at HttpApiSchemaError.cause.issue; format them explicitly when a flat external list is needed.

- `HttpApiError.TypeId` -> `effect/http-api/HttpApiError#HttpApiSchemaErrorTypeId`: The old module symbol is gone; prefer HttpApiSchemaError.is for runtime narrowing.

## `@effect/platform/HttpApiGroup`

- `HttpApiGroup.ApiGroup` -> `effect/http-api/HttpApiGroup#Service`: Renamed; the service field and type parameter are now identifier rather than name.

- `HttpApiGroup.HttpApiGroup.AddContext` -> `none`: Groups no longer carry arbitrary context. Use AddMiddleware for middleware service transformations.

- `HttpApiGroup.HttpApiGroup.Any` -> `effect/http-api/HttpApiGroup#Constraint`: Renamed widened structural constraint.

- `HttpApiGroup.HttpApiGroup.AnyWithProps` -> `effect/http-api/HttpApiGroup#Top`: Renamed widened runtime-property type.

- `HttpApiGroup.HttpApiGroup.ClientContext` -> `effect/http-api/HttpApiGroup#ClientServices / ErrorServicesDecode / MiddlewareClient`: Client schema services and required client middleware are separate extractors in v4.

- `HttpApiGroup.HttpApiGroup.Context` -> `none`: Group error and context generics were removed; derive server requirements from the group's endpoints.

- `HttpApiGroup.HttpApiGroup.ContextWithName` -> `none`: Select with WithIdentifier and derive endpoint server requirements; groups no longer have a context generic.

- `HttpApiGroup.HttpApiGroup.EndpointsWithName` -> `effect/http-api/HttpApiGroup#EndpointsWithIdentifier`: Direct rename from name to identifier.

- `HttpApiGroup.HttpApiGroup.Error` -> `none`: Group-level errors were removed. Declare shared errors on each endpoint or through middleware.

- `HttpApiGroup.HttpApiGroup.ErrorContext` -> `effect/http-api/HttpApiGroup#ErrorServicesEncode / ErrorServicesDecode`: The closest endpoint-error aggregate splits server encoding from client decoding services.

- `HttpApiGroup.HttpApiGroup.ErrorWithName` -> `none`: Group-level errors were removed; select with WithIdentifier and inspect Errors over the selected endpoints.

- `HttpApiGroup.HttpApiGroup.Middleware` -> `effect/http-api/HttpApiEndpoint#Middleware`: Middleware is attached to the endpoints present when group.middleware is called; extract it from group endpoints.

- `HttpApiGroup.HttpApiGroup.MiddlewareWithName` -> `effect/http-api/HttpApiEndpoint#Middleware`: Select the group with WithIdentifier, get its endpoints, then apply the endpoint Middleware extractor.

- `HttpApiGroup.HttpApiGroup.Provides` -> `effect/http-api/HttpApiGroup#MiddlewareProvides`: Renamed; derives provided services from endpoint middleware.

- `HttpApiGroup.HttpApiGroup.ToService` -> `effect/http-api/HttpApiGroup#ToService`: Same role and now produces Service\<ApiId, Identifier\>.

- `HttpApiGroup.HttpApiGroup.WithName` -> `effect/http-api/HttpApiGroup#WithIdentifier`: Direct rename from name to identifier.

- `HttpApiGroup.TypeId` -> `none`: The group type ID is private; use HttpApiGroup.isHttpApiGroup for runtime narrowing.

- `HttpApiGroup.make` -> `effect/http-api/HttpApiGroup#make`: The constructor remains; group error and context generics are gone and add is variadic.

## `@effect/platform/HttpApiMiddleware`

- `HttpApiMiddleware.HttpApiMiddleware` -> `effect/http-api/HttpApiMiddleware#HttpApiMiddleware`: The model remains but now wraps the response effect and carries provided services, an error schema, and required services.

- `HttpApiMiddleware.HttpApiMiddleware.Any` -> `effect/http-api/HttpApiMiddleware#AnyService`: Renamed widened middleware service-key shape.

- `HttpApiMiddleware.HttpApiMiddleware.AnyId` -> `effect/http-api/HttpApiMiddleware#AnyId`: Same name; metadata now includes provided and required services, error schema, client error, and client requirement.

- `HttpApiMiddleware.HttpApiMiddleware.Error` -> `effect/http-api/HttpApiMiddleware#Error`: Same name and now derives the decoded type from the configured error schema.

- `HttpApiMiddleware.HttpApiMiddleware.ErrorContext` -> `effect/http-api/HttpApiMiddleware#ErrorServicesEncode / ErrorServicesDecode`: The single schema context split into server encoding and client decoding services.

- `HttpApiMiddleware.HttpApiMiddleware.Only` -> `Extract<R, HttpApiMiddleware.AnyId>`: The helper was removed because middleware IDs are explicit; use Extract when the direct filter is still needed.

- `HttpApiMiddleware.HttpApiMiddleware.Provides` -> `effect/http-api/HttpApiMiddleware#Provides`: Same name and reads the expanded v4 middleware ID metadata.

- `HttpApiMiddleware.SecurityTypeId` -> `none`: The marker is private; use HttpApiMiddleware.isSecurity.

- `HttpApiMiddleware.Tag` -> `effect/http-api/HttpApiMiddleware#Service`: Renamed and redesigned; use error, requires, provides, clientError, and requiredForClient configuration.

- `HttpApiMiddleware.TagClass` -> `effect/http-api/HttpApiMiddleware#ServiceClass`: Renamed class type with the new two-stage type configuration and wrapping service shape.

- `HttpApiMiddleware.TagClass.BaseSecurity` -> `effect/http-api/HttpApiMiddleware#ServiceClass`: Security is conditional metadata on ServiceClass; there is no separate public base interface.

- `HttpApiMiddleware.TagClass.Failure` -> `effect/http-api/HttpApiMiddleware#Error`: failure terminology became error; apply the extractor to the middleware ID.

- `HttpApiMiddleware.TagClass.FailureContext` -> `effect/http-api/HttpApiMiddleware#ErrorServicesEncode / ErrorServicesDecode`: Failure schema services split by server encoding and client decoding direction.

- `HttpApiMiddleware.TagClass.FailureSchema` -> `effect/http-api/HttpApiMiddleware#ErrorSchema`: Renamed and applied to the middleware ID rather than constructor options.

- `HttpApiMiddleware.TagClass.FailureService` -> `effect/http-api/HttpApiMiddleware#Error`: Use the decoded error extractor; optional middleware fallback was removed.

- `HttpApiMiddleware.TagClass.Optional` -> `none`: Optional declaration and fallback-on-failure behavior were removed; model fallback in the wrapping middleware.

- `HttpApiMiddleware.TagClass.Provides` -> `effect/http-api/HttpApiMiddleware#Provides`: Moved to the module level and applied to the middleware ID.

- `HttpApiMiddleware.TagClassAny` -> `effect/http-api/HttpApiMiddleware#AnyService`: Renamed widened service-key type.

- `HttpApiMiddleware.TagClassSecurityAny` -> `effect/http-api/HttpApiMiddleware#AnyServiceSecurity`: Renamed widened security service-key type.

- `HttpApiMiddleware.TypeId` -> `none`: The marker is private; use public guards and type extractors.

## `@effect/platform/HttpApiScalar`

- `HttpApiScalar.layer` -> `effect/http-api/HttpApiScalar#layer`: Pass the HttpApi as the first argument; the layer now contributes directly to HttpRouter.

- `HttpApiScalar.layerHttpLayerRouter` -> `effect/http-api/HttpApiScalar#layer`: The duplicate was removed. Pass options.api as the first layer argument and the remaining Scalar options second.

- `HttpApiScalar.layerHttpLayerRouterCdn` -> `effect/http-api/HttpApiScalar#layerCdn`: Use the explicit-api CDN layer with path, version, and Scalar options.

## `@effect/platform/HttpApiSchema`

- `HttpApiSchema.AnnotationEmptyDecodeable` -> `effect/http-api/HttpApiSchema#asNoContent`: The public marker was removed; represent no-content decoding structurally with asNoContent({ decode }).

- `HttpApiSchema.AnnotationEncoding` -> `effect/http-api/HttpApiSchema#asJson / asFormUrlEncoded / asText / asUint8Array`: The key is internal; select encoding with a public combinator.

- `HttpApiSchema.AnnotationMultipart` -> `effect/http-api/HttpApiSchema#asMultipart`: The symbol annotation became a brand plus internal encoding metadata; apply the schema combinator.

- `HttpApiSchema.AnnotationMultipartStream` -> `effect/http-api/HttpApiSchema#asMultipartStream`: The symbol annotation became a brand plus internal encoding metadata; apply the schema combinator.

- `HttpApiSchema.AnnotationParam` -> `effect/http-api/HttpApiEndpoint#params`: Path names now live in the router path and schemas in endpoint option params, not schema annotations.

- `HttpApiSchema.AnnotationStatus` -> `effect/http-api/HttpApiSchema#status`: The public symbol was removed; apply status(code), which uses the httpApiStatus schema annotation.

- `HttpApiSchema.Empty` -> `effect/http-api/HttpApiSchema#Empty`: The API remains and returns Schema.Void annotated with the supplied status.

- `HttpApiSchema.EmptyError` -> `effect/Schema#Error`: Define a normal schema error with httpApiStatus, then derive its no-content wire schema with asNoContent.

- `HttpApiSchema.EmptyErrorClass` -> `effect/Schema#Error`: The class and no-content codec are separate in v4; combine Schema.Error with HttpApiSchema.asNoContent.

- `HttpApiSchema.EmptyErrorUnify` -> `none`: Removed with EmptyError; Schema.Error instances already support yieldable-error behavior.

- `HttpApiSchema.EmptyErrorUnifyIgnore` -> `none`: Removed with EmptyError; do not recreate the old Unify marker.

- `HttpApiSchema.Encoding` -> `effect/http-api/HttpApiSchema#Encoding`: The name remains but is now a discriminated PayloadEncoding or ResponseEncoding union; prefer public as\* combinators.

- `HttpApiSchema.Multipart` -> `effect/http-api/HttpApiSchema#asMultipart`: The type and constructor became a curried schema combinator: schema.pipe(asMultipart(options)).

- `HttpApiSchema.MultipartStream` -> `effect/http-api/HttpApiSchema#asMultipartStream`: The type and constructor became a curried schema combinator.

- `HttpApiSchema.Text` -> `effect/http-api/HttpApiSchema#asText`: Apply the encoding combinator to Schema.String instead of using a dedicated constructor.

- `HttpApiSchema.Uint8Array` -> `effect/http-api/HttpApiSchema#asUint8Array`: Apply the encoding combinator to Schema.Uint8Array instead of using a dedicated constructor.

- `HttpApiSchema.UnionUnify` -> `effect/Schema#Union`: Use Schema.Union([self, that]); for endpoint alternatives, pass the schema array directly to preserve metadata.

- `HttpApiSchema.annotations` -> `effect/Schema#annotate`: Schema annotations became annotate; set httpApiStatus directly or prefer HttpApiSchema.status for status only.

- `HttpApiSchema.asEmpty` -> `effect/http-api/HttpApiSchema#asNoContent`: Use schema.pipe(asNoContent({ decode }), status(code)); status is now a separate combinator.

- `HttpApiSchema.deunionize` -> `none`: Pass schema arrays to endpoint success, error, and body alternatives so each member retains status and content type.

- `HttpApiSchema.extractAnnotations` -> `none`: The internal symbol-copy helper was removed; HTTP metadata is schema-native and resolved through AST traversal.

- `HttpApiSchema.getEmptyDecodeable` -> `effect/http-api/HttpApiSchema#isNoContent`: Use isNoContent only to test bodylessness; decodeability is structural and has no exact query replacement.

- `HttpApiSchema.getEncoding` -> `effect/http-api/HttpApiSchema#getPayloadEncoding / getResponseEncoding`: Encoding lookup split by direction; application code should normally use public as\* combinators.

- `HttpApiSchema.getMultipart` -> `effect/http-api/HttpApiSchema#getPayloadEncoding`: Narrow the payload encoding to Multipart with buffered mode; multipart limits are on the encoding value.

- `HttpApiSchema.getMultipartStream` -> `effect/http-api/HttpApiSchema#getPayloadEncoding`: Narrow the payload encoding to Multipart with stream mode; multipart limits are on the encoding value.

- `HttpApiSchema.getParam` -> `none`: Param identity moved out of schema metadata; read endpoint.path and endpoint.params.

- `HttpApiSchema.getStatus` -> `effect/SchemaAST#resolveAt`: Resolve the httpApiStatus annotation directly, or prefer getStatusSuccess and getStatusError for response logic.

- `HttpApiSchema.getStatusError` -> `effect/http-api/HttpApiSchema#getStatusError`: The helper remains but accepts an AST and defaults to 500.

- `HttpApiSchema.getStatusErrorAST` -> `effect/http-api/HttpApiSchema#getStatusError`: The AST suffix collapsed into the sole helper, which defaults to 500.

- `HttpApiSchema.getStatusSuccess` -> `effect/http-api/HttpApiSchema#getStatusSuccess`: The helper remains but accepts an AST; bare Schema.Void now defaults to 200, so use Empty(204) for 204.

- `HttpApiSchema.getStatusSuccessAST` -> `effect/http-api/HttpApiSchema#getStatusSuccess`: The AST suffix collapsed into the sole helper; bare Schema.Void no longer implies 204.

- `HttpApiSchema.param` -> `effect/http-api/HttpApiEndpoint#params`: Use a literal /:name path and the matching field in endpoint constructor option params.

- `HttpApiSchema.withEncoding` -> `effect/http-api/HttpApiSchema#asJson / asFormUrlEncoded / asUint8Array / asText`: Replace the generic kind with the matching public curried encoding combinator.

## `@effect/platform/HttpApiSecurity`

- `HttpApiSecurity.Bearer` -> `effect/http-api/HttpApiSecurity#Http`: Bearer was generalized to Http with scheme Bearer; the value-level bearer singleton remains.

- `HttpApiSecurity.TypeId` -> `none`: The marker is private; use the public union or specific Http, ApiKey, and Basic types.

- `HttpApiSecurity.annotate` -> `effect/http-api/HttpApiSecurity#annotate`: The combinator remains; its key is now the v4 Context.Key abstraction.

- `HttpApiSecurity.annotateContext` -> `effect/http-api/HttpApiSecurity#annotateMerge`: Renamed; it still merges a Context into existing OpenAPI annotations.

## `@effect/platform/HttpApiSwagger`

- `HttpApiSwagger.layer` -> `effect/http-api/HttpApiSwagger#layer`: Pass the HttpApi as the first argument; the layer now contributes directly to HttpRouter.

- `HttpApiSwagger.layerHttpLayerRouter` -> `effect/http-api/HttpApiSwagger#layer`: The duplicate was removed. Pass options.api first and the path option second.

## `@effect/platform/HttpApp`

- `HttpApp.Default` -> `Effect.Effect<HttpServerResponse.HttpServerResponse, E, R | HttpServerRequest.HttpServerRequest>`: The alias was removed; v4 HTTP applications are ordinary response-producing Effects.

- `HttpApp.HttpApp` -> `Effect.Effect<A, E, R | HttpServerRequest.HttpServerRequest>`: The alias was removed; use the underlying Effect type and HttpEffect boundary combinators.

- `HttpApp.currentPreResponseHandlers` -> `HttpEffect.appendPreResponseHandler / HttpEffect.withPreResponseHandler`: The FiberRef was removed; register request-local handlers through HttpEffect.

- `HttpApp.ejectDefaultScopeClose` -> `HttpEffect.scopeDisableClose`: Renamed; it disables automatic request-scope closure, leaving closure to the caller.

- `HttpApp.toWebHandler` -> `HttpEffect.toWebHandler`: Moved to HttpEffect for converting an HTTP effect to a Web handler.

- `HttpApp.toWebHandlerRuntime` -> `HttpEffect.toWebHandlerWith(context)`: Runtime was removed in v4; supply a Context with toWebHandlerWith instead.

- `HttpApp.unsafeEjectStreamScope` -> `HttpEffect.scopeTransferToStream`: Renamed; it transfers request-scope closure to a streaming response.

## `@effect/platform/HttpBody`

- `HttpBody.Empty` -> `HttpBody.Empty`: Retained with the same tag, but v4 exports a class rather than an interface.

- `HttpBody.ErrorReason` -> `HttpBody.ErrorReason`: Retained but reshaped; original causes now live on HttpBodyError.cause.

- `HttpBody.ErrorTypeId` -> `HttpBody.HttpBodyError`: The error type-id is private in v4; identify the exported error class instead.

- `HttpBody.HttpBodyError` -> `HttpBody.HttpBodyError`: Changed from a factory/interface to a class constructed with reason and optional cause.

- `HttpBody.Raw` -> `HttpBody.Raw`: Retained with the same tag and payload, but v4 exports a class.

- `HttpBody.Stream` -> `HttpBody.Stream`: Retained with the same tag and byte stream, but v4 exports a class.

- `HttpBody.TypeId` -> `HttpBody.isHttpBody`: The body brand is private in v4; use the public refinement instead.

- `HttpBody.Uint8Array` -> `HttpBody.Uint8Array`: Retained with the same fields and tag, but v4 exports a class.

- `HttpBody.file` -> `HttpBody.file`: Retained; bufferSize was replaced by numeric chunkSize. Offset and bytesToRead accept ByteSize.Input. Invalid ranges and a final EOF-clamped content length above Number.MAX\_SAFE\_INTEGER fail with PlatformError / BadArgument.

- `HttpBody.fileInfo` -> `HttpBody.fileFromInfo`: Renamed; it uses supplied File.Info with ByteSize size metadata and requires FileSystem. Offset and bytesToRead accept ByteSize.Input, while chunkSize is numeric. Invalid ranges and a final EOF-clamped content length above Number.MAX\_SAFE\_INTEGER fail with PlatformError / BadArgument.

- `HttpBody.unsafeJson` -> `HttpBody.jsonUnsafe`: Renamed to put Unsafe last; serialization failures still throw.

- `HttpBody.urlParams` -> `HttpBody.urlParams`: Retained and widened to accept UrlParams.Input.

## `@effect/platform/HttpClient`

- `HttpClient.SpanNameGenerator` -> `HttpClient.SpanNameGenerator`: The interface became a Context.Reference containing the generator function.

- `HttpClient.TypeId` -> `HttpClient.isHttpClient`: The brand key is private in v4; use the public runtime refinement.

- `HttpClient.catchAll` -> `HttpClient.catch`: Renamed to catch; the recovery callback still returns a response effect.

- `HttpClient.catchTag` -> `HttpClient.catchTag`: Retained and widened to accept one or more error tags.

- `HttpClient.currentTracerDisabledWhen` -> `HttpClient.TracerDisabledWhen`: Renamed and changed from FiberRef to Context.Reference.

- `HttpClient.currentTracerPropagation` -> `HttpClient.TracerPropagationEnabled`: Renamed and changed from FiberRef to Context.Reference\<boolean\>.

- `HttpClient.filterOrFail` -> `HttpClient.filterOrFail`: Retained; v4 also provides refinement overloads.

- `HttpClient.filterStatus` -> `HttpClient.filterStatus`: Retained; rejection now fails with the HttpClientError wrapper.

- `HttpClient.filterStatusOk` -> `HttpClient.filterStatusOk`: Retained; non-2xx responses now fail with the HttpClientError wrapper.

- `HttpClient.make` -> `HttpClient.make`: Retained; the runner receives Fiber.Fiber and failures use the v4 error wrapper.

- `HttpClient.makeWith` -> `HttpClient.makeWith`: Retained with the preprocess and postprocess constructor pattern.

- `HttpClient.retry` -> `HttpClient.retry`: Retained; the Schedule error channel is included in the resulting client error type.

- `HttpClient.tap` -> `effect/http/HttpClient#tap`: Moved to the v4 HTTP module with the same response-effect callback and client error/service widening.

- `HttpClient.transform` -> `effect/http/HttpClient#transform`: Moved to the v4 HTTP module with the same request-aware transformation shape.

- `HttpClient.withSpanNameGenerator` -> `HttpClient.transformResponse(Effect.provideService(HttpClient.SpanNameGenerator, f))`: The convenience combinator was removed; provide the reference around response effects.

- `HttpClient.withTracerDisabledWhen` -> `HttpClient.transformResponse(Effect.provideService(HttpClient.TracerDisabledWhen, predicate))`: The convenience combinator was removed; provide the reference around response effects.

- `HttpClient.withTracerPropagation` -> `HttpClient.transformResponse(Effect.provideService(HttpClient.TracerPropagationEnabled, enabled))`: Provide the renamed propagation reference around response effects.

## `@effect/platform/HttpClientError`

- `HttpClientError.HttpClientError` -> `HttpClientError.HttpClientError`: Changed from a union to a tagged wrapper class containing a concrete failure in reason.

- `HttpClientError.RequestError` -> `HttpClientError.RequestError`: Now a type-only reason union; construct a concrete reason and wrap it in HttpClientError.

- `HttpClientError.TypeId` -> `HttpClientError.isHttpClientError`: The brand key is private in v4; use the public runtime refinement.

## `@effect/platform/HttpClientRequest`

- `HttpClientRequest.Options.NoBody` -> `HttpClientRequest.Options.NoUrl`: NoBody was removed; v4 method helpers uniformly omit only url.

- `HttpClientRequest.TypeId` -> `HttpClientRequest.isHttpClientRequest`: The request brand is private in v4; use the public runtime refinement.

- `HttpClientRequest.bodyFileWeb` -> `HttpClientRequest.setBody + HttpBody.stream + Stream.fromReadableStream`: No one-call replacement remains; stream file.stream() and pass file.type and file.size to HttpBody.stream.

- `HttpClientRequest.bodyUnsafeJson` -> `HttpClientRequest.bodyJsonUnsafe`: Renamed to put Unsafe last; serialization remains synchronous and throwing.

- `HttpClientRequest.get` -> `HttpClientRequest.get`: Retained; options now use Options.NoUrl and no longer exclude body.

- `HttpClientRequest.head` -> `HttpClientRequest.head`: Retained; options now use Options.NoUrl and no longer exclude body.

- `HttpClientRequest.make` -> `HttpClientRequest.make`: Retained; all methods now accept Options.NoUrl without the GET/HEAD body restriction.

- `HttpClientRequest.setBody` -> `HttpClientRequest.setBody`: Retained and still synchronizes body content metadata into headers.

- `HttpClientRequest.toUrl` -> `HttpClientRequest.toUrl`: Retained and still returns Option\<URL\>.

## `@effect/platform/HttpClientResponse`

- `HttpClientResponse.HttpClientResponse` -> `HttpClientResponse.HttpClientResponse`: Custom implementations must supply the required url string. It represents the resolved URL including query parameters and excluding the hash, uses the final URL after redirects, and is empty when unknown.

- `HttpClientResponse.TypeId` -> `typeof HttpClientResponse.TypeId`: TypeId remains public but is now a string constant; use typeof in type position.

- `HttpClientResponse.filterStatus` -> `HttpClientResponse.filterStatus`: Retained; rejected status now fails with an HttpClientError wrapper.

- `HttpClientResponse.filterStatusOk` -> `HttpClientResponse.filterStatusOk`: Retained; non-2xx status now fails with an HttpClientError wrapper.

- `HttpClientResponse.schemaBodyJson` -> `HttpClientResponse.schemaBodyJson`: Retained with v4 Schema constraints and SchemaError failures.

- `HttpClientResponse.schemaBodyUrlParams` -> `HttpClientResponse.schemaBodyUrlParams`: Retained with ConstraintCodec input and SchemaError failures.

- `HttpClientResponse.schemaHeaders` -> `HttpClientResponse.schemaHeaders`: Retained with ConstraintCodec input and SchemaError failures.

- `HttpClientResponse.schemaJson` -> `HttpClientResponse.schemaJson`: Retained with ConstraintCodec input and v4 error types.

- `HttpClientResponse.schemaNoBody` -> `HttpClientResponse.schemaNoBody`: Retained with Schema.Codec input and SchemaError failures.

- `HttpClientResponse.stream` -> `HttpClientResponse.stream`: Retained; body failures now use the broader HttpClientError wrapper.

## `@effect/platform/HttpIncomingMessage`

- `HttpIncomingMessage.MaxBodySize` -> `HttpIncomingMessage.MaxBodySize`: Changed from a Reference subclass holding Option\<Size\> to Context.Reference\<ByteSize.ByteSize | undefined\>.

- `HttpIncomingMessage.TypeId` -> `typeof HttpIncomingMessage.TypeId`: TypeId remains public but is now a string constant; use typeof in type position.

- `HttpIncomingMessage.withMaxBodySize` -> `Effect.provideService(HttpIncomingMessage.MaxBodySize, size)`: The helper was removed; provide a ByteSize value or undefined directly.

## `@effect/platform/HttpLayerRouter`

- `HttpLayerRouter.FindMyWay.PathInput` -> `FindMyWay.PathInput`: Import FindMyWay from effect/http.

- `HttpLayerRouter.FindMyWay.make` -> `FindMyWay.make`: Import FindMyWay from effect/http.

- `HttpLayerRouter.MiddlewareTypeId` -> `none`: The middleware type id is internal in v4; use HttpRouter.Middleware.

- `HttpLayerRouter.PathInput` -> `HttpRouter.PathInput`: Moved to the consolidated v4 router.

- `HttpLayerRouter.Request.From` -> `HttpRouter.Request.From`: Moved with the layer-oriented router into the consolidated HttpRouter module.

- `HttpLayerRouter.Request.Only` -> `HttpRouter.Request.Only`: Moved with the layer-oriented router into the consolidated HttpRouter module.

- `HttpLayerRouter.Route.Context` -> `HttpRouter.Route.Context`: Moved with the Route helper types into the consolidated HttpRouter module.

- `HttpLayerRouter.Route.Error` -> `HttpRouter.Route.Error`: Moved with the Route helper types into the consolidated HttpRouter module.

- `HttpLayerRouter.RouteContext` -> `HttpRouter.RouteContext`: Moved to the consolidated v4 router.

- `HttpLayerRouter.RouteTypeId` -> `none`: Route nominal ids are internal in v4; construct routes with HttpRouter.route.

- `HttpLayerRouter.RouterConfig` -> `HttpRouter.RouterConfig`: Now a Context.Reference containing Partial\<FindMyWay.RouterConfig\>.

- `HttpLayerRouter.TypeId` -> `none`: The router nominal service id is internal in v4; use HttpRouter.HttpRouter.

- `HttpLayerRouter.add` -> `HttpRouter.add`: Moved to the consolidated HttpRouter; it still returns a route-registration Layer.

- `HttpLayerRouter.addAll` -> `HttpRouter.addAll`: Moved to the consolidated HttpRouter; it still registers route values through a Layer and supports a prefix option.

- `HttpLayerRouter.addHttpApi` -> `HttpApiBuilder.layer`: HTTP API registration moved to effect/http-api.

- `HttpLayerRouter.cors` -> `HttpRouter.cors`: HttpLayerRouter was consolidated into v4 HttpRouter.

- `HttpLayerRouter.layer` -> `HttpRouter.layer`: Use the layer for the consolidated HttpRouter service.

- `HttpLayerRouter.make` -> `HttpRouter.make`: The layer-oriented router became the sole v4 HttpRouter implementation.

- `HttpLayerRouter.schemaJson` -> `HttpRouter.schemaJson`: Moved to the consolidated router with v4 Schema and error types.

- `HttpLayerRouter.schemaNoBody` -> `HttpRouter.schemaNoBody`: Moved to the consolidated router with v4 Schema types.

- `HttpLayerRouter.serve` -> `HttpRouter.serve`: Moved to the consolidated router; pass the route-registration layer.

- `HttpLayerRouter.toHttpEffect` -> `HttpRouter.toHttpEffect`: Moved to the consolidated HttpRouter; route-not-found failures now use HttpServerError.HttpServerError.

- `HttpLayerRouter.toWebHandler` -> `HttpRouter.toWebHandler`: Moved to the consolidated router for building a Fetch handler and disposer.

## `@effect/platform/HttpMiddleware`

- `HttpMiddleware.SpanNameGenerator` -> `HttpMiddleware.SpanNameGenerator`: The branded interface became a Context.Reference containing the generator.

- `HttpMiddleware.cors` -> `HttpMiddleware.cors`: Retained with the same CORS options and behavior.

- `HttpMiddleware.currentTracerDisabledWhen` -> `HttpMiddleware.TracerDisabledWhen`: The FiberRef became a Context.Reference containing the request predicate.

- `HttpMiddleware.loggerDisabled` -> `HttpMiddleware.withLoggerDisabled`: The FiberRef was removed; locally wrap an effect or use HttpRouter.disableLogger.

- `HttpMiddleware.withSpanNameGenerator` -> `Layer.provide(layer, Layer.succeed(HttpMiddleware.SpanNameGenerator)(f))`: Provide the SpanNameGenerator reference to the target layer.

- `HttpMiddleware.withTracerDisabledForUrls` -> `Layer.provide(layer, HttpMiddleware.layerTracerDisabledForUrls(urls))`: Provide the new URL-predicate layer to the target layer.

- `HttpMiddleware.withTracerDisabledWhen` -> `Layer.provide(layer, Layer.succeed(HttpMiddleware.TracerDisabledWhen)(predicate))`: Provide the TracerDisabledWhen reference to the target layer.

- `HttpMiddleware.withTracerDisabledWhenEffect` -> `Effect.provideService(effect, HttpMiddleware.TracerDisabledWhen, predicate)`: Provide the TracerDisabledWhen reference locally to the effect.

## `@effect/platform/HttpPlatform`

- `HttpPlatform.HttpPlatform` -> `HttpPlatform.HttpPlatform`: The service is now a Context.Service class; use its Service member for the implementation type. Path-backed offset and bytesToRead accept ByteSize.Input, while chunkSize and all Web File range options use number.

- `HttpPlatform.TypeId` -> `none`: The public type id was removed; use the HttpPlatform Context.Service class.

- `HttpPlatform.layer` -> `HttpPlatform.layer`: Retained as the default file-response layer.

- `HttpPlatform.make` -> `HttpPlatform.make`: Retained; v4 returns the service implementation. The fileResponse callback receives contentLength as bigint, while start and end remain numbers. Web File range options use number.

## `@effect/platform/HttpRouter`

- `HttpRouter.Default` -> `HttpRouter.HttpRouter + HttpRouter.layer`: Custom/default router tags were removed; v4 provides one router service.

- `HttpRouter.HttpRouter` -> `HttpRouter.HttpRouter`: The name remains, but now denotes the mutable layer-oriented registration service.

- `HttpRouter.HttpRouter.DefaultServices` -> `none`: The custom tagged-router default-service bundle was removed.

- `HttpRouter.HttpRouter.Service` -> `HttpRouter.HttpRouter`: Use the consolidated router service interface.

- `HttpRouter.Route.Middleware` -> `Effect.Effect`: Spell the route response Effect directly, or use HttpRouter.middleware for transforms.

- `HttpRouter.RouteContextTypeId` -> `none`: The nominal id is internal in v4; access HttpRouter.RouteContext as a service.

- `HttpRouter.RouteTypeId` -> `none`: The nominal id is internal in v4; construct routes with HttpRouter.route.

- `HttpRouter.Tag` -> `none`: Custom router tags were removed; use the singleton router service and registration layers.

- `HttpRouter.TypeId` -> `none`: The router nominal service id is internal in v4; use HttpRouter.HttpRouter.

- `HttpRouter.all` -> `HttpRouter.add("*", path, handler, options)`: v4 registers a route layer instead of returning an immutable router.

- `HttpRouter.append` -> `HttpRouter.addAll([route])`: Register the route and merge its layer with other route layers.

- `HttpRouter.catchAll` -> `HttpRouter.middleware + Effect.catch`: Apply typed-error recovery in route middleware provided to route layers.

- `HttpRouter.catchAllCause` -> `HttpRouter.middleware + Effect.catchCause`: Apply cause recovery in route middleware provided to route layers.

- `HttpRouter.catchTag` -> `HttpRouter.middleware + Effect.catchTag`: Apply tagged-error recovery in route middleware provided to route layers.

- `HttpRouter.concat` -> `Layer.merge`: Routers are now route-registration layers; merge the two layers.

- `HttpRouter.concatAll` -> `Layer.mergeAll`: Routers are now route-registration layers; merge all layers.

- `HttpRouter.currentRouterConfig` -> `HttpRouter.RouterConfig`: The FiberRef became a Context.Reference containing Partial\<FindMyWay.RouterConfig\>.

- `HttpRouter.empty` -> `Layer.empty`: There is no immutable empty router; use an empty registration layer.

- `HttpRouter.fromIterable` -> `HttpRouter.addAll(Array.from(routes))`: Materialize and register the route descriptors as a layer.

- `HttpRouter.get` -> `HttpRouter.add("GET", path, handler, options)`: Register a route layer; handlers must produce HttpServerResponse.

- `HttpRouter.head` -> `HttpRouter.addAll([HttpRouter.route("HEAD", path, handler, options)])`: Use route plus addAll because add does not expose HEAD.

- `HttpRouter.makeRoute` -> `HttpRouter.route`: Renamed to route; v4 route options no longer expose the old prefix field.

- `HttpRouter.mount` -> `HttpRouter.addAll(routes, { prefix: path })`: Register child routes with a prefix, or use router.prefixed(path).

- `HttpRouter.mountApp` -> `HttpRouter.use((router) => router.prefixed(path).add("*", "/*", app))`: Register the app on the prefixed router service; no direct mount API remains.

- `HttpRouter.options` -> `HttpRouter.add("OPTIONS", path, handler, options)`: Register a route layer; handlers must produce HttpServerResponse.

- `HttpRouter.patch` -> `HttpRouter.add("PATCH", path, handler, options)`: Register a route layer; handlers must produce HttpServerResponse.

- `HttpRouter.post` -> `HttpRouter.add("POST", path, handler, options)`: Register a route layer; handlers must produce HttpServerResponse.

- `HttpRouter.prefixAll` -> `HttpRouter.addAll(routes, { prefix })`: Apply the prefix while registering route descriptors.

- `HttpRouter.put` -> `HttpRouter.add("PUT", path, handler, options)`: Register a route layer; handlers must produce HttpServerResponse.

- `HttpRouter.setRouterConfig` -> `Layer.succeed(HttpRouter.RouterConfig)(config)`: Provide the RouterConfig Context.Reference as a layer.

- `HttpRouter.toHttpApp` -> `HttpRouter.toHttpEffect`: Pass the route-registration layer to build the server handler effect.

- `HttpRouter.transform` -> `HttpRouter.middleware`: Express the route-wide response Effect transform as router middleware.

- `HttpRouter.withRouterConfig` -> `Effect.provideService(effect, HttpRouter.RouterConfig, config)`: Provide the RouterConfig Context.Reference locally instead of setting a FiberRef.

## `@effect/platform/HttpServer`

- `HttpServer.Address` -> `effect/net/NetAddress#SocketAddress`: Replaced by the shared concrete internet-or-Unix socket address union.

- `HttpServer.HttpServer` -> `HttpServer.HttpServer`: The interface and tag became one Context.Service class; use its Service member for implementations.

- `HttpServer.ServeOptions` -> `none`: The unused respond option model was removed with no shared v4 counterpart.

- `HttpServer.TcpAddress` -> `effect/net/NetAddress#InetAddress`: Replaced by the shared resolved internet-address model; use address and port instead of hostname and port.

- `HttpServer.TypeId` -> `none`: The public TypeId was removed; HttpServer is now a Context.Service class.

- `HttpServer.UnixAddress` -> `effect/net/NetAddress#UnixPathAddress`: Replaced by the shared Unix filesystem-path address model.

- `HttpServer.addressWith` -> `HttpServer.HttpServer.use(({ address }) => effect(address))`: The accessor was removed; read the service and pass its Address to the callback.

- `HttpServer.formatAddress` -> `HttpServer.formatAddress`: Accepts NetAddress.SocketAddress and delegates to NetAddress.formatUrlUnsafe. Scoped IPv6 addresses throw NetAddressError; Unix paths use the unix:// prefix.

- `HttpServer.layerContext` -> `HttpServer.layerServices`: Renamed; it provides the standard HTTP platform services.

- `HttpServer.make` -> `HttpServer.make`: Retained; it returns the Context.Service implementation. Supply a NetAddress.SocketAddress for address, using NetAddress.inetAddressUnsafe for a parsed IP and port or NetAddress.unixPathAddress for a Unix path. Use NetAddress.inetAddress to validate the port through Result instead.

- `HttpServer.serve` -> `effect/http/HttpServer#serve`: Moved to the v4 HTTP module; the application is now an Effect producing HttpServerResponse rather than the separate HttpApp model.

## `@effect/platform/HttpServerError`

- `HttpServerError.HttpServerError` -> `HttpServerError.HttpServerError | HttpServerError.ServeError`: Handler failures became a tagged wrapper, while ServeError remains separate.

- `HttpServerError.RequestError` -> `HttpServerError.RequestParseError (constructor) / HttpServerError.RequestError (type)`: The constructible class became RequestParseError; RequestError is now a broader type union.

- `HttpServerError.TypeId` -> `HttpServerError.isHttpServerError`: The brand is private in v4; use the public runtime refinement.

- `HttpServerError.clientAbortFiberId` -> `HttpServerError.ClientAbort.annotation`: Client aborts now use a Cause context annotation rather than a sentinel FiberId.

- `HttpServerError.isServerError` -> `HttpServerError.isHttpServerError`: Renamed and narrowed to wrapped handler errors; test ServeError separately if needed.

## `@effect/platform/HttpServerRequest`

- `HttpServerRequest.ParsedSearchParams` -> `HttpServerRequest.ParsedSearchParams`: The marker and tag became one Context.Service class.

- `HttpServerRequest.TypeId` -> `typeof HttpServerRequest.TypeId`: TypeId remains public but is now a string constant; use typeof in type position.

- `HttpServerRequest.fromWeb` -> `HttpServerRequest.fromWeb`: Retained for wrapping a Web Request.

- `HttpServerRequest.persistedMultipart` -> `HttpServerRequest.HttpServerRequest.use((request) => request.multipart)`: Use the request service's `.use` helper to return its cached multipart effect.

- `HttpServerRequest.schemaBodyJson` -> `HttpServerRequest.schemaBodyJson`: Retained with v4 Schema constraints and error types.

- `HttpServerRequest.schemaBodyUrlParams` -> `HttpServerRequest.schemaBodyUrlParams`: Retained with ConstraintCodec input and v4 error types.

- `HttpServerRequest.schemaHeaders` -> `HttpServerRequest.schemaHeaders`: Retained with ConstraintCodec input and SchemaError failures.

- `HttpServerRequest.toWeb` -> `HttpServerRequest.toWeb`: Retained and captures the current Context for streamed bodies.

- `HttpServerRequest.toWebEither` -> `HttpServerRequest.toWebResult`: Either became Result, and the optional Runtime became an optional Context.

## `@effect/platform/HttpServerRespondable`

- `HttpServerRespondable.symbol` -> `HttpServerRespondable.symbol`: Retained as a string protocol key rather than a unique symbol.

## `@effect/platform/HttpServerResponse`

- `HttpServerResponse.TypeId` -> `HttpServerResponse.isHttpServerResponse`: The response brand is private in v4; use the public runtime refinement.

- `HttpServerResponse.expireCookie` -> `HttpServerResponse.expireCookie`: Now effectful and safe; use expireCookieUnsafe for synchronous throwing behavior.

- `HttpServerResponse.file` -> `HttpServerResponse.file`: Retained; offset and bytesToRead accept ByteSize.Input, while chunkSize uses number. Path-backed responses validate ranges and clamp content length to the available bytes.

- `HttpServerResponse.fileWeb` -> `HttpServerResponse.fileWeb`: Web File offset, bytesToRead, and chunkSize options use number, unlike path-backed ByteSize.Input ranges.

- `HttpServerResponse.isServerResponse` -> `HttpServerResponse.isHttpServerResponse`: Renamed.

- `HttpServerResponse.setCookie` -> `HttpServerResponse.setCookie`: Retained as the safe effectful cookie setter.

- `HttpServerResponse.stream` -> `HttpServerResponse.stream`: Retained; v4 Stream no longer has a service type parameter.

- `HttpServerResponse.text` -> `HttpServerResponse.text`: Moved unchanged.

- `HttpServerResponse.toWeb` -> `HttpServerResponse.toWeb`: Retained, but the optional Runtime became an optional Context for stream execution. For a raw Web Response, outer headers override native headers and cookies append, including when the body is omitted. With withoutBody, the raw status and statusText are preserved unless the outer status is 204, 205, or 304, which takes precedence.

- `HttpServerResponse.uint8Array` -> `HttpServerResponse.uint8Array`: Moved unchanged.

- `HttpServerResponse.unsafeJson` -> `HttpServerResponse.jsonUnsafe`: Renamed to put Unsafe last; serialization failures still throw.

- `HttpServerResponse.unsafeSetCookie` -> `HttpServerResponse.setCookieUnsafe`: Renamed to put Unsafe last; invalid cookies still throw.

- `HttpServerResponse.unsafeSetCookies` -> `HttpServerResponse.setCookiesUnsafe`: Renamed to put Unsafe last; invalid cookies still throw.

## `@effect/platform/KeyValueStore`

- `KeyValueStore.KeyValueStore` -> `KeyValueStore.KeyValueStore`: The service moved to effect/persistence/KeyValueStore; missing values now use undefined and operations fail with KeyValueStoreError.

- `KeyValueStore.KeyValueStore.AnyStore` -> `KeyValueStore.KeyValueStore | KeyValueStore.SchemaStore<Schema.Constraint>`: The convenience namespace alias was removed; write the store union explicitly when needed.

- `KeyValueStore.SchemaStoreTypeId` -> `none`: The v4 SchemaStore has no public type-id alias; use the SchemaStore interface.

- `KeyValueStore.TypeId` -> `none`: The KeyValueStore runtime marker is internal in v4; use the service and interface.

- `KeyValueStore.layerSchema` -> `KeyValueStore.toSchemaStore`: Schema stores are now derived with toSchemaStore; define the desired Context.Service and layer explicitly.

- `KeyValueStore.make` -> `KeyValueStore.make`: The constructor remains in the moved module with v4 MakeOptions.

- `KeyValueStore.prefix` -> `KeyValueStore.prefix`: The prefixed-store combinator remains in the moved module.

## `@effect/platform/MsgPack`

- `MsgPack.ErrorTypeId` -> `Schema.SchemaError`: MsgPackError was removed with MessagePack. SchemaBinary channels fail with Schema.SchemaError.

- `MsgPack.MsgPackError` -> `Schema.SchemaError`: MsgPackError was removed with MessagePack. SchemaBinary channels fail with Schema.SchemaError.

- `MsgPack.duplex` -> `SchemaBinary.duplex`: MessagePack was removed. Wrap a byte channel with SchemaBinary.duplex and explicit input and output schemas.

- `MsgPack.duplexSchema` -> `SchemaBinary.duplex`: MessagePack was removed. Use SchemaBinary.duplex with the v4 Schema model.

- `MsgPack.pack` -> `SchemaBinary.encode`: MessagePack was removed. Encode schema values to binary frames with SchemaBinary.encode.

- `MsgPack.packSchema` -> `SchemaBinary.encode`: MessagePack was removed. Use SchemaBinary.encode with the v4 Schema model.

- `MsgPack.schema` -> `SchemaBinary.toCodec`: MessagePack was removed. Derive a binary codec from a Schema with SchemaBinary.toCodec.

- `MsgPack.unpack` -> `SchemaBinary.decode`: MessagePack was removed. Decode binary frames with SchemaBinary.decode.

- `MsgPack.unpackSchema` -> `SchemaBinary.decode`: MessagePack was removed. Use SchemaBinary.decode with the v4 Schema model.

## `@effect/platform/Multipart`

- `Multipart.ErrorTypeId` -> `Multipart.MultipartError`: The public error type-id alias was removed; use the MultipartError class.

- `Multipart.FieldMimeTypes` -> `Multipart.FieldMimeTypes`: The setting remains but is now a Context.Reference rather than a service class.

- `Multipart.FileSchema` -> `Multipart.PersistedFileSchema`: The schema for persisted multipart files was renamed.

- `Multipart.MaxFieldSize` -> `Multipart.MaxFieldSize`: Now a Context.Reference\<ByteSize.ByteSize\>; provide a value such as ByteSize.bytes(100).

- `Multipart.MaxFileSize` -> `Multipart.MaxFileSize`: Now a Context.Reference\<ByteSize.ByteSize | undefined\>; provide ByteSize.bytes(100), for example, or undefined for no limit.

- `Multipart.MaxParts` -> `Multipart.MaxParts`: The setting remains as a Context.Reference; use undefined rather than Option.none for no limit.

- `Multipart.TypeId` -> `typeof Multipart.TypeId`: The runtime marker remains exported, but the separate type alias was removed.

- `Multipart.makeChannel` -> `effect/http/Multipart#makeChannel`: The channel constructor moved and no longer accepts bufferSize; input and output chunks use non-empty readonly arrays.

- `Multipart.schemaJson` -> `Multipart.schemaJson`: The JSON-field decoder remains in effect/http/Multipart and uses v4 Schema constraints.

- `Multipart.withFieldMimeTypes` -> `Effect.provideService(Multipart.FieldMimeTypes, mimeTypes)`: Provide the v4 Context.Reference around the effect.

- `Multipart.withLimits` -> `Effect.provideContext(effect, Multipart.limitsServices(options))`: Build the multipart limit context and provide it to the effect; Option-valued limits became optional plain values.

- `Multipart.withLimits.Options` -> `Multipart.withLimits.Options`: Limit fields now use optional plain numbers or ByteSize inputs; convert Option.none to undefined and Option.some(value) to value.

- `Multipart.withLimitsStream` -> `Stream.provideContext(stream, Multipart.limitsServices(options))`: Build the multipart limit context and provide it to the stream; Option-valued limits became optional plain values.

- `Multipart.withMaxFieldSize` -> `Effect.provideService(Multipart.MaxFieldSize, size)`: Provide a ByteSize value, such as ByteSize.bytes(100). To normalize ByteSize.Input options, use Multipart.limitsServices.

- `Multipart.withMaxFileSize` -> `Effect.provideService(Multipart.MaxFileSize, size)`: Replace Option.none with undefined and Option.some(value) with a normalized ByteSize value. To normalize ByteSize.Input options, use Multipart.limitsServices.

- `Multipart.withMaxParts` -> `Effect.provideService(Multipart.MaxParts, count)`: Provide the v4 Context.Reference around the effect, converting Option.none to undefined.

## `@effect/platform/Ndjson`

- `Ndjson.ErrorTypeId` -> `Ndjson.NdjsonError`: The public error marker was removed; use the NdjsonError class.

- `Ndjson.NdjsonErrorTypeId` -> `Ndjson.NdjsonError`: The public error type-id alias was removed; use the NdjsonError class.

- `Ndjson.NdjsonOptions` -> `{ readonly ignoreEmptyLines?: boolean }`: The standalone options interface was removed; decoding and duplex APIs accept this inline shape.

- `Ndjson.duplex` -> `Ndjson.duplex`: The API moved to effect/encoding/Ndjson.

- `Ndjson.duplexSchema` -> `Ndjson.duplexSchema`: The API moved to effect/encoding/Ndjson and uses v4 Schema constraints.

- `Ndjson.pack` -> `Ndjson.encode`: The NDJSON channel constructor was renamed from pack to encode.

- `Ndjson.packSchema` -> `Ndjson.encodeSchema`: The schema-aware pack channel was renamed to encodeSchema.

- `Ndjson.packSchemaString` -> `Ndjson.encodeSchemaString`: The string schema pack channel was renamed to encodeSchemaString.

- `Ndjson.packString` -> `Ndjson.encodeString`: The string pack channel was renamed to encodeString.

- `Ndjson.unpack` -> `Ndjson.decode`: The NDJSON channel constructor was renamed from unpack to decode.

- `Ndjson.unpackSchema` -> `Ndjson.decodeSchema`: The schema-aware unpack channel was renamed to decodeSchema.

- `Ndjson.unpackSchemaString` -> `Ndjson.decodeSchemaString`: The string schema unpack channel was renamed to decodeSchemaString.

- `Ndjson.unpackString` -> `Ndjson.decodeString`: The string unpack channel was renamed to decodeString.

## `@effect/platform/OpenApi`

- `OpenApi.AdditionalPropertiesStrategy` -> `none`: OpenApi.fromApi no longer accepts generation options; standalone JSON Schema generation has a separate additionalProperties option.

- `OpenApi.Exclude` -> `effect/http-api/OpenApi#Exclude`: Same annotation key and default; it is now a Context.Reference value.

- `OpenApi.OpenApiSpecContentType` -> `string`: The closed media-type union was removed so custom and streaming media types are supported.

- `OpenApi.fromApi` -> `effect/http-api/OpenApi#fromApi`: The operation remains and returns OpenAPI 3.1, but the signature is now only fromApi(api).

## `@effect/platform/OpenApiJsonSchema`

- `OpenApiJsonSchema.Any` -> `effect/JsonSchema#JsonSchema`: The narrow node interfaces were consolidated into the open, dialect-neutral JSON Schema object model.

- `OpenApiJsonSchema.AnyObject` -> `effect/JsonSchema#JsonSchema`: The narrow node interfaces were consolidated; construct the required object directly.

- `OpenApiJsonSchema.AnyOf` -> `effect/JsonSchema#JsonSchema`: The narrow node interfaces were consolidated into the general object model.

- `OpenApiJsonSchema.Array` -> `effect/JsonSchema#JsonSchema`: The narrow node interfaces were consolidated into the general object model.

- `OpenApiJsonSchema.Boolean` -> `effect/JsonSchema#JsonSchema`: The narrow boolean interface was consolidated into the open, dialect-neutral JSON Schema object model.

- `OpenApiJsonSchema.Empty` -> `effect/JsonSchema#JsonSchema`: The narrow node interfaces and special id shapes were removed.

- `OpenApiJsonSchema.Enum` -> `effect/JsonSchema#JsonSchema`: The narrow node interfaces were consolidated into the general object model.

- `OpenApiJsonSchema.Enums` -> `effect/JsonSchema#JsonSchema`: The Effect-specific comment enum shape has no named v4 interface; use the general object model.

- `OpenApiJsonSchema.Integer` -> `effect/JsonSchema#JsonSchema`: The narrow numeric interfaces were consolidated into the general object model.

- `OpenApiJsonSchema.JsonSchema` -> `effect/JsonSchema#JsonSchema`: Use the dialect-neutral open JSON Schema object model.

- `OpenApiJsonSchema.Never` -> `effect/JsonSchema#JsonSchema`: The special never-schema interface was consolidated into the open JSON Schema object model; represent it with a not constraint.

- `OpenApiJsonSchema.Number` -> `effect/JsonSchema#JsonSchema`: The narrow number interface was consolidated into the open, dialect-neutral JSON Schema object model.

- `OpenApiJsonSchema.Numeric` -> `effect/JsonSchema#JsonSchema`: The narrow numeric interfaces were consolidated into the general object model.

- `OpenApiJsonSchema.Object` -> `effect/JsonSchema#JsonSchema`: The narrow node interfaces were consolidated into the general object model.

- `OpenApiJsonSchema.Ref` -> `effect/JsonSchema#JsonSchema`: The narrow ref interface was consolidated; OpenAPI conversion rewrites definition references.

- `OpenApiJsonSchema.Root` -> `effect/JsonSchema#MultiDocument`: OpenAPI generation keeps roots in schemas and shared components in definitions; the inline-definitions root model is gone.

- `OpenApiJsonSchema.String` -> `effect/JsonSchema#JsonSchema`: The narrow string interface was consolidated into the open, dialect-neutral JSON Schema object model.

- `OpenApiJsonSchema.Unknown`: TODO: needs guidance

- `OpenApiJsonSchema.Void` -> `effect/JsonSchema#JsonSchema`: The special void-schema interface was consolidated into the open JSON Schema object model.

- `OpenApiJsonSchema.make` -> `effect/Schema#toJsonSchemaDocument + effect/JsonSchema#toMultiDocumentOpenApi3_1`: Generate Draft 2020-12, wrap the root in a multi-document, then convert references and definitions to OpenAPI 3.1.

- `OpenApiJsonSchema.makeWithDefs` -> `effect/SchemaRepresentation#toJsonSchemaMultiDocument + effect/JsonSchema#toMultiDocumentOpenApi3_1`: Definitions are returned separately; build a multi-document representation and convert it to OpenAPI 3.1.

## `@effect/platform/Path`

- `Path.TypeId` -> `typeof Path.TypeId`: The module moved to effect/Path; the runtime marker remains exported but the separate type alias was removed.

## `@effect/platform/PlatformConfigProvider`

- `PlatformConfigProvider.fromFileTree` -> `ConfigProvider.fromDir`: The provider moved into effect/ConfigProvider and was renamed; rootDirectory is now rootPath.

- `PlatformConfigProvider.layerDotEnv` -> `ConfigProvider.layer(ConfigProvider.fromDotEnv({ path }))`: Use the v4 dotenv provider effect and install it with ConfigProvider.layer.

- `PlatformConfigProvider.layerDotEnvAdd` -> `ConfigProvider.layerAdd(ConfigProvider.fromDotEnv({ path }))`: Use the v4 dotenv provider effect and compose it with ConfigProvider.layerAdd.

- `PlatformConfigProvider.layerFileTree` -> `ConfigProvider.layer(ConfigProvider.fromDir({ rootPath }))`: Use the renamed directory-tree provider and install it with ConfigProvider.layer.

- `PlatformConfigProvider.layerFileTreeAdd` -> `ConfigProvider.layerAdd(ConfigProvider.fromDir({ rootPath }))`: Use the renamed directory-tree provider and compose it with ConfigProvider.layerAdd.

## `@effect/platform/Runtime`

- `Runtime.RunMain` -> `ReturnType<typeof Runtime.makeRunMain>`: The standalone interface was removed; derive the runner type from effect/Runtime.makeRunMain. disablePrettyLogger is no longer an option.

## `@effect/platform/Socket`

- `Socket.CloseEventTypeId` -> `Socket.CloseEvent`: The close-event marker is internal in v4; use the CloseEvent class or Socket.isCloseEvent.

- `Socket.SocketError` -> `Socket.SocketError`: The old union became a tagged wrapper around SocketReadError, SocketWriteError, SocketOpenError, or SocketCloseError.

- `Socket.SocketErrorTypeId` -> `Socket.SocketErrorTypeId`: The error marker remains in effect/socket/Socket.

- `Socket.SocketGenericError` -> `Socket.SocketReadError | Socket.SocketWriteError | Socket.SocketOpenError`: The generic reason discriminator was replaced by dedicated read, write, and open error classes.

- `Socket.TypeId` -> `typeof Socket.TypeId`: The socket marker remains exported, but the separate type alias was removed.

- `Socket.WebSocket` -> `Socket.WebSocket`: The opaque service moved to effect/socket/Socket and is now a Context.Service class for globalThis.WebSocket.

- `Socket.WebSocketConstructor` -> `Socket.WebSocketConstructor`: The service moved to effect/socket/Socket and is now a Context.Service class.

- `Socket.currentSendQueueCapacity` -> `none`: The send queue was removed. The v4 Socket is pull-based: acquire socket.reader in a scope and pull frame batches; writes apply the transport's native backpressure.

- `Socket.defaultCloseCodeIsError` -> `none`: Sockets no longer classify close codes; every close fails the reader's pull with a SocketError wrapping SocketCloseError. Consumers that treat a close as normal catch the error.

- `Socket.fromTransformStream` -> `Socket.fromTransformStream`: The constructor remains in effect/socket/Socket but drops closeCodeIsError; every close fails the reader's pull with a SocketError wrapping SocketCloseError.

- `Socket.layerWebSocket` -> `Socket.layerWebSocket`: The constructor remains in effect/socket/Socket; its URL may now also be an Effect.

- `Socket.toChannelMap` -> `none`: The v4 Socket read side is an Effect that never completes via Cause.Done; map frames by acquiring Socket.readerBytes or Socket.readerString, or Effect.map the reader from socket.reader, and use Socket.toChannel or Socket.toChannelString for duplex channels.

## `@effect/platform/SocketServer`

- `SocketServer.Address` -> `effect/net/NetAddress#SocketAddress`: Replaced by the shared concrete internet-or-Unix socket address union.

- `SocketServer.ErrorTypeId` -> `SocketServer.ErrorTypeId`: The API moved to effect/socket/SocketServer and retains this name.

- `SocketServer.TcpAddress` -> `effect/net/NetAddress#InetAddress`: Replaced by the shared resolved internet-address model; use address and port instead of hostname and port.

- `SocketServer.UnixAddress` -> `effect/net/NetAddress#UnixPathAddress`: Replaced by the shared Unix filesystem-path address model.

## `@effect/platform/Template`

- `Template.Interpolated.Context` -> `Template.Interpolated.Context`: The API moved to effect/http/Template; v4 interpolation types also account for Effect values.

- `Template.Interpolated.Error` -> `Template.Interpolated.Error`: The API moved to effect/http/Template; v4 interpolation types also account for Effect values.

## `@effect/platform/Terminal`

- `Terminal.QuitException` -> `Terminal.QuitError`: The quit sentinel was renamed and moved to effect/Terminal.

- `Terminal.isQuitException` -> `Terminal.isQuitError`: The quit sentinel was renamed from QuitException to QuitError.

## `@effect/platform/Transferable`

- `Transferable.CollectorService` -> `Transferable.Collector["Service"]`: The collector interface is now the service type of the Transferable.Collector Context.Service class.

- `Transferable.Uint8Array` -> `Transferable.Uint8Array`: The transferable Uint8Array schema remains in the moved module.

- `Transferable.schema` -> `Transferable.schema`: The schema wrapper moved to effect/workers/Transferable and uses the v4 Schema model.

- `Transferable.unsafeMakeCollector` -> `Transferable.makeCollectorUnsafe`: The unsafe collector constructor was renamed.

## `@effect/platform/Url`

- `Url.fromString` -> `Url.fromString`: Retained; returns Result with IllegalArgumentError instead of Either with IllegalArgumentException.

- `Url.setUrlParams` -> `Url.setUrlParams`: Retained and widened to accept UrlParams.Input.

## `@effect/platform/UrlParams`

- `UrlParams.CoercibleRecord` -> `UrlParams.CoercibleRecord<A>`: The recursive interface became a generic mapped type preserving the input shape.

- `UrlParams.Input` -> `UrlParams.Input`: Retained and broadened to include UrlParams itself.

- `UrlParams.UrlParams` -> `UrlParams.UrlParams`: Import UrlParams from effect/http. It is now a branded iterable object with a params field rather than a ReadonlyArray; construct it with UrlParams.make or UrlParams.fromInput.

- `UrlParams.makeUrl` -> `Url.make`: Moved to Url, returns Result, and takes string | undefined for the hash.

- `UrlParams.schemaFromSelf` -> `Schema.UrlParams`: The declaration schema for the v4 wrapper moved to effect/Schema.

- `UrlParams.schemaFromString` -> `Schema.String.pipe(Schema.decodeTo(Schema.UrlParams, { decode: SchemaGetter.transform((s) => UrlParams.fromInput(new URLSearchParams(s))), encode: SchemaGetter.transform(UrlParams.toString) }))`: No prebuilt string codec remains; recreate it by transforming between a query string and UrlParams.

- `UrlParams.schemaJson` -> `Schema.JsonFromUrlParamsField(field).pipe(Schema.decodeTo(schema), Schema.decodeEffect)`: The field codec moved to effect/Schema. Compose it with the target schema, then decode it.

- `UrlParams.schemaParse` -> `UrlParamsFromString.pipe(Schema.decodeTo(Schema.RecordFromUrlParams.pipe(Schema.decodeTo(schema))))`: Recreate the removed helper by composing the string, record, and target codecs.

- `UrlParams.schemaRecord` -> `Schema.RecordFromUrlParams.pipe(Schema.decodeTo(schema))`: RecordFromUrlParams is a base codec in effect/Schema; compose it with the target schema.

- `UrlParams.schemaStruct` -> `Schema.RecordFromUrlParams.pipe(Schema.decodeTo(schema), Schema.decodeEffect)`: Compose the record codec from effect/Schema with the target schema and decode it.

- `UrlParams.toString` -> `UrlParams.toString`: Retained and broadened to accept any UrlParams.Input.

## `@effect/platform/Worker`

- `Worker.BackingWorker` -> `Worker.Worker`: The low-level backing worker became the primary Worker interface with send and run operations.

- `Worker.PlatformWorker` -> `Worker.WorkerPlatform`: The platform service was renamed and is now a Context.Service class.

- `Worker.PlatformWorkerTypeId` -> `none`: The Context.Service class replaces the public platform-worker type-id alias.

- `Worker.SerializedWorker` -> `RpcClient with RpcClient.layerProtocolWorker`: The serialized worker facade was removed; v4 routes schema-defined RPCs through the worker protocol.

- `Worker.SerializedWorker.Options` -> `RpcWorker.layerInitialMessage`: Use RpcWorker.layerInitialMessage when a worker RPC protocol needs schema-encoded initialization.

- `Worker.SerializedWorkerPool` -> `RpcClient.makeProtocolWorker`: The worker-backed RPC protocol owns its worker pool in v4.

- `Worker.SerializedWorkerPool.Options` -> `Parameters<typeof RpcClient.makeProtocolWorker>[0]`: Pool sizing options moved to the worker RPC protocol; initial messages are provided separately with RpcWorker.layerInitialMessage.

- `Worker.Worker` -> `Worker.Worker`: The name remains in effect/workers/Worker, but it is now the low-level send/run abstraction rather than execute/executeEffect.

- `Worker.Worker.Options` -> `Worker.Worker["run"] options`: Encoding moved to RPC schemas; the low-level run operation only accepts an optional onSpawn effect.

- `Worker.Worker.Response` -> `none`: The old tagged-request wire response is gone; worker RPC wire messages are internal to RpcClient and RpcServer.

- `Worker.Worker.Span` -> `none`: The explicit span tuple was removed; the RPC worker protocol handles span propagation internally.

- `Worker.WorkerManager` -> `Worker.WorkerPlatform`: WorkerPlatform now spawns low-level Worker values directly, replacing WorkerManager.

- `Worker.WorkerManagerTypeId` -> `none`: The removed WorkerManager has no v4 type-id; WorkerPlatform is a Context.Service class.

- `Worker.WorkerPool` -> `RpcClient.Protocol`: For serialized request/response workloads use the worker-backed RPC Protocol; for raw messages build a Pool around WorkerPlatform.spawn.

- `Worker.WorkerPool.Options` -> `Parameters<typeof RpcClient.makeProtocolWorker>[0]`: Worker RPC pool sizing is configured on makeProtocolWorker or layerProtocolWorker.

- `Worker.layerManager` -> `Worker.WorkerPlatform`: WorkerManager was removed; provide the adapter's WorkerPlatform layer directly.

- `Worker.makeManager` -> `Worker.WorkerPlatform`: WorkerManager was removed; obtain WorkerPlatform and call its spawn method.

- `Worker.makePool` -> `Pool + Worker.WorkerPlatform.spawn`: Generic worker pools are no longer built by this module; build a Pool around WorkerPlatform.spawn, or use RpcClient.makeProtocolWorker for RPC workers.

- `Worker.makePoolLayer` -> `RpcClient.layerProtocolWorker`: The standard v4 worker-pool layer is the worker-backed RPC client protocol; compose it with the RPC client layer.

- `Worker.makePoolSerialized` -> `RpcClient.makeProtocolWorker`: Serialized tagged-request workers were replaced by the worker-backed RPC protocol.

- `Worker.makePoolSerializedLayer` -> `RpcClient.layerProtocolWorker`: Serialized tagged-request worker pools were replaced by the worker-backed RPC protocol layer.

- `Worker.makeSerialized` -> `RpcClient with RpcClient.layerProtocolWorker`: Serialized tagged-request execution moved to the v4 RPC model; define an RpcGroup and use the worker protocol.

## `@effect/platform/WorkerError`

- `WorkerError.WorkerErrorFrom` -> `WorkerError.WorkerError`: The old serializable reason object was replaced by WorkerError wrapping dedicated spawn, send, receive, or unknown reason classes.

- `WorkerError.WorkerErrorTypeId` -> `WorkerError.TypeId`: The type-level worker error marker was shortened to TypeId in the moved module.

## `@effect/platform/WorkerRunner`

- `WorkerRunner.BackingRunner` -> `WorkerRunner.WorkerRunner`: The low-level backing runner became the primary WorkerRunner interface.

- `WorkerRunner.BackingRunner.Message` -> `WorkerRunner.PlatformMessage`: The request/close wire tuple moved to the top-level PlatformMessage type.

- `WorkerRunner.CloseLatch` -> `none`: The public close-latch service was removed; WorkerRunner implementations manage lifetime through their run effect and adapter scope.

- `WorkerRunner.PlatformRunner` -> `WorkerRunner.WorkerRunnerPlatform`: The platform service was renamed and is now a Context.Service class.

- `WorkerRunner.PlatformRunnerTypeId` -> `none`: The Context.Service class replaces the public platform-runner type-id alias.

- `WorkerRunner.Runner` -> `WorkerRunner.WorkerRunner`: The namespace-only runner API was replaced by the low-level WorkerRunner interface.

- `WorkerRunner.Runner.Options` -> `none`: The custom decode/encode callbacks were removed; use raw low-level messages or define schemas in the v4 RPC model.

- `WorkerRunner.SerializedRunner` -> `RpcServer with RpcGroup handlers`: The serialized runner namespace was removed in favor of typed Rpc definitions and RpcServer.

- `WorkerRunner.SerializedRunner.Handlers` -> `RpcGroup.HandlersFrom`: Define an RpcGroup and derive its server handler object type with HandlersFrom.

- `WorkerRunner.SerializedRunner.HandlersContext` -> `RpcGroup.HandlersServices`: Derive services required by an RpcGroup handler object with HandlersServices.

- `WorkerRunner.SerializedRunner.InitialContext` -> `none`: Initial-message layer outputs are no longer inferred by this helper; model initialization as normal RpcGroup handler layers and services.

- `WorkerRunner.SerializedRunner.InitialEnv` -> `none`: Initial-message layer inputs are no longer inferred by this helper; model initialization as normal RpcGroup handler layers and services.

- `WorkerRunner.launch` -> `RpcServer.layerProtocolWorkerRunner`: For schema-defined workers, provide the worker-runner RPC protocol and launch the normal RpcServer layer.

- `WorkerRunner.layer` -> `WorkerRunner.WorkerRunnerPlatform.start + WorkerRunner.WorkerRunner.run`: The generic processing layer was removed; use the low-level runner directly or the RpcServer worker protocol.

- `WorkerRunner.layerCloseLatch` -> `none`: The public close-latch layer was removed; adapter runner lifetime is managed internally.

- `WorkerRunner.layerSerialized` -> `RpcServer.layerProtocolWorkerRunner`: Serialized tagged-request handlers moved to RpcGroup handlers served through the worker-runner RPC protocol.

- `WorkerRunner.make` -> `WorkerRunner.WorkerRunnerPlatform.start + WorkerRunner.WorkerRunner.run`: Start the platform runner and register the low-level message handler directly.

- `WorkerRunner.makeSerialized` -> `RpcServer.makeProtocolWorkerRunner`: Serialized tagged-request execution moved to RpcServer with an RpcGroup handler layer.
