# v3 → v4 API map: @effect/platform-node (7 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/platform-node/NodeCommandExecutor`

- `NodeCommandExecutor.layer` -> `NodeChildProcessSpawner.layer`: CommandExecutor became ChildProcessSpawner; use the @effect/platform-node/NodeChildProcessSpawner re-export.

## `@effect/platform-node/NodeContext`

- `NodeContext.NodeContext` -> `NodeServices.NodeServices`: Use the renamed service union; it replaces CommandExecutor with ChildProcessSpawner, adds Crypto and Stdio, and omits WorkerManager.

- `NodeContext.layer` -> `NodeServices.layer`: The aggregate was renamed and now provides ChildProcessSpawner, Crypto, FileSystem, Path, Stdio, and Terminal; add NodeWorker separately when needed.

## `@effect/platform-node/NodeFileSystem/ParcelWatcher`

- `ParcelWatcher.layer` -> `NodeFileSystem.layer`: The Parcel watcher adapter was removed. Native node:fs.watch support is built in; FileSystem.WatchBackend is the extension point.

## `@effect/platform-node/NodeHttpClient`

- `NodeHttpClient.Dispatcher` -> `NodeHttpClient.Dispatcher`: The identifier remains but is now a Context.Service class; use Dispatcher["Service"] for the concrete Undici dispatcher type.

- `NodeHttpClient.HttpAgent` -> `NodeHttpClient.HttpAgent`: The identifier remains but is now a Context.Service class; use HttpAgent["Service"] for the concrete http/https agent pair.

- `NodeHttpClient.HttpAgentTypeId` -> `none`: The public marker was removed; the HttpAgent Context.Service class supplies service identity.

- `NodeHttpClient.UndiciRequestOptions` -> `NodeHttpClient.UndiciOptions`: The required Context.Tag became a defaulted Context.Reference\<Partial\<Dispatcher.RequestOptions\>\>; override it with Effect.provideService.

- `NodeHttpClient.agentLayer` -> `NodeHttpClient.layerAgent`: Direct rename; it provides the default scoped Node HTTP and HTTPS agents.

- `NodeHttpClient.dispatcherLayer` -> `NodeHttpClient.layerDispatcher`: Direct rename; the layer owns and finalizes a scoped Undici Agent.

- `NodeHttpClient.layer` -> `NodeHttpClient.layerNodeHttp`: Use the renamed node:http/node:https backend layer; choose layerUndici only when intentionally changing backends.

- `NodeHttpClient.layerUndiciWithoutDispatcher` -> `NodeHttpClient.layerUndiciNoDispatcher`: Direct rename; the layer still requires NodeHttpClient.Dispatcher.

- `NodeHttpClient.layerWithoutAgent` -> `NodeHttpClient.layerNodeHttpNoAgent`: Direct rename; the node:http client layer still requires NodeHttpClient.HttpAgent.

- `NodeHttpClient.make` -> `NodeHttpClient.makeNodeHttp`: Direct rename of the node:http/node:https client constructor.

- `NodeHttpClient.makeAgentLayer` -> `NodeHttpClient.layerAgentOptions`: Direct rename; it accepts Https.AgentOptions and scopes both agents.

## `@effect/platform-node/NodeHttpServer`

- `NodeHttpServer.layerContext` -> `NodeHttpServer.layerHttpServices`: Direct rename; it provides NodeServices, HttpPlatform, and Etag.Generator, without the removed WorkerManager.

## `@effect/platform-node/NodeWorker`

- `NodeWorker.layerManager` -> `NodeWorker.layerPlatform`: WorkerManager was removed. Provide WorkerPlatform directly, or use NodeWorker.layer(spawn) when a Worker.Spawner is also required.

- `NodeWorker.layerWorker` -> `NodeWorker.layerPlatform`: PlatformWorker became Worker.WorkerPlatform; NodeWorker.layer(spawn) combines the platform and spawner layers.

## `@effect/platform-node/Undici`

- `Undici.Agent` -> `undici.Agent`: Import the upstream Agent directly. Undici 8 removes maxRedirections and option-level interceptors, adds maxOrigins, and enables HTTP/2 negotiation unless allowH2 is false.

- `Undici.Agent.DispatchOptions` -> `undici.Agent.DispatchOptions`: Import the same Agent namespace type from undici; Undici 8 removes maxRedirections.

- `Undici.Agent.Options` -> `undici.Agent.Options`: Import the same Agent namespace type; Undici 8 removes maxRedirections and option-level interceptors, adds maxOrigins, and uses dispatcher.compose for interceptors.

- `Undici.Client` -> `undici.Client`: Import the upstream Client directly; custom handlers must use Undici 8's controller-based v2 handler API.

- `Undici.Client.Options` -> `undici.Client.Options`: Import the same Client namespace type; Undici 8 removes maxRedirections and option-level interceptors and adds WebSocket and HTTP/2 options.

- `Undici.Client.OptionsInterceptors` -> `undici.Dispatcher.DispatcherComposeInterceptor + dispatcher.compose`: Undici 8 removed option-level interceptor tuples; keep DispatcherComposeInterceptor functions and apply them after construction with dispatcher.compose(...).

- `Undici.DiagnosticsChannel` -> `undici.DiagnosticsChannel`: Import this type-only namespace from undici; subscribe at runtime through node:diagnostics\_channel using Undici's channel names.

- `Undici.DiagnosticsChannel.ClientConnectErrorMessage` -> `undici.DiagnosticsChannel.ClientConnectErrorMessage`: Import the same type-only namespace member from undici; runtime delivery uses node:diagnostics\_channel.

- `Undici.DiagnosticsChannel.Error` -> `Error`: Undici 8 removed this unknown alias; diagnostic error fields now use the built-in Error type.

- `Undici.DiagnosticsChannel.RequestErrorMessage` -> `undici.DiagnosticsChannel.RequestErrorMessage`: Import the same type-only namespace member; its error field is the built-in Error type in Undici 8.

- `Undici.DiagnosticsChannel.Response`: TODO: needs guidance

- `Undici.Dispatcher` -> `undici.Dispatcher`: Import the upstream Dispatcher directly; custom dispatchers must adopt Undici 8's controller-based v2 handler API.

- `Undici.Dispatcher.ConnectOptions` -> `undici.Dispatcher.ConnectOptions`: Import the same Dispatcher namespace type; Undici 8 removes maxRedirections and redirectionLimitReached.

- `Undici.Dispatcher.DispatchHandler` -> `undici.Dispatcher.DispatchHandler`: Use Undici 8's onRequestStart/onResponseStart/onResponseData/onResponseEnd/onResponseError callbacks and controller pause/resume/abort methods.

- `Undici.Dispatcher.DispatchOptions` -> `undici.Dispatcher.DispatchOptions`: Import the same namespace type; Undici 8 removes throwOnError, adds typeOfService, and handles redirects through composed interceptors.

- `Undici.Dispatcher.RequestOptions` -> `undici.Dispatcher.RequestOptions`: Import the same namespace type; Undici 8 removes maxRedirections and redirectionLimitReached, so compose a redirect interceptor when needed.

- `Undici.Dispatcher.UpgradeOptions` -> `undici.Dispatcher.UpgradeOptions`: Import the same namespace type; Undici 8 removes maxRedirections and redirectionLimitReached.

- `Undici.H2CClient` -> `undici.H2CClient`: Import the upstream cleartext HTTP/2 client directly; callbacks follow Undici 8's handler API.

- `Undici.H2CClient.Options` -> `undici.H2CClient.Options`: Import the same H2CClient namespace type; Undici 8 removes maxRedirections.

- `Undici.MessageEvent` -> `undici.MessageEvent`: Import Undici's named constructor/type directly to preserve the installed package identity.

- `Undici.MessageEventInit` -> `undici.MessageEventInit`: Import the upstream type directly; message ports and source use MessagePort instances in Undici 8.

- `Undici.MockAgent`: TODO: needs guidance

- `Undici.Pool` -> `undici.Pool`: Import the upstream Pool directly and apply interceptors after construction with pool.compose(...).

- `Undici.Pool.Options` -> `undici.Pool.Options`: Import the same Pool namespace type; Undici 8 removes the interceptors option in favor of pool.compose(...).

- `Undici.ProxyAgent` -> `undici.ProxyAgent`: Import the upstream ProxyAgent directly; inherited Agent options and handlers follow Undici 8.

- `Undici.ProxyAgent.Options` -> `undici.ProxyAgent.Options`: Import the same ProxyAgent namespace type; Undici 8 types proxy headers as OutgoingHttpHeaders.

- `Undici.RedirectHandler` -> `undici.RedirectHandler`: Import the upstream class; Undici 8 removes redirectionLimitReached from the constructor and adds static buildDispatch.

- `Undici.Request` -> `undici.Request`: Import Undici's named Request directly; clone is a method in Undici 8.

- `Undici.Response` -> `undici.Response`: Import Undici's named Response directly; clone is a method and Response.redirect status is optional in Undici 8.

- `Undici.SpecIterable` -> `undici.SpecIterable`: Import the upstream type directly; its iterator returns SpecIterableIterator in Undici 8.

- `Undici.SpecIterableIterator` -> `undici.SpecIterableIterator`: Import the upstream type directly; it extends SpecIteratorObject and includes iterator-helper methods in Undici 8.

- `Undici.buildConnector` -> `undici.buildConnector`: Import the upstream function directly; Undici 8 expands connector options with preferH2, typeOfService, and socketPath handling.

- `Undici.buildConnector.BuildOptions` -> `undici.buildConnector.BuildOptions`: Import the same buildConnector namespace type; Undici 8 adds preferH2 and typeOfService.

- `Undici.default.cacheStores` -> `undici.cacheStores`: Use Undici 8's named cacheStores export instead of reaching through the default aggregate.

- `Undici.deleteCookie` -> `undici.deleteCookie`: Import the upstream function directly; its optional attributes use path and domain and no longer include name.

- `Undici.errors` -> `undici.errors`: Import the upstream errors object directly; individual classes follow the Undici 8 API.

- `Undici.errors.ResponseStatusCodeError` -> `undici.errors.ResponseError`: Undici 8 replaced ResponseStatusCodeError with ResponseError; construct it with message, statusCode, and the headers/body object.

- `Undici.interceptors` -> `undici.interceptors`: Import the upstream interceptors object and apply returned interceptors with dispatcher.compose(...).

- `Undici.interceptors.DNSInterceptorOpts` -> `undici.interceptors.DNSInterceptorOpts`: Import the same namespace type; Undici 8 lookup receives an origin URL and supports optional DNS storage.

- `Undici.interceptors.DNSInterceptorOriginRecords` -> `undici.interceptors.DNSInterceptorOriginRecords`: Import the same namespace type, but adopt Undici 8's shape with IPv4 and IPv6 entries nested under records.

- `Undici.interceptors.RedirectInterceptorOpts` -> `undici.interceptors.RedirectInterceptorOpts`: Import the same namespace type; Undici 8 adds throwOnMaxRedirect and redirect header-stripping options.
