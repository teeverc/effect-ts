# HTTP Server (`effect/http-api` + `effect/http`)

Use this guide when defining, serving, testing, or calling HTTP APIs. Official examples:

- `references/examples/51_http-server/10_basics.ts` — serving, docs, web handler, typed client
- `references/examples/51_http-server/20_testing.ts` — `HttpApiTest`
- `references/examples/51_http-server/fixtures/` — the split between `api/`, `server/`, and `domain/`

```ts
import { HttpApi, HttpApiBuilder, HttpApiClient, HttpApiEndpoint, HttpApiError, HttpApiGroup,
  HttpApiMiddleware, HttpApiScalar, HttpApiSchema, HttpApiSecurity, HttpApiTest, OpenApi } from "effect/http-api"
import { HttpRouter, HttpServer, HttpServerResponse, HttpMiddleware, HttpStaticServer } from "effect/http"
import { NodeHttpServer, NodeRuntime } from "@effect/platform-node"
```

Most of these modules are `@stability unstable`.

## Mental model

1. **Define** the API as schemas only: `HttpApiEndpoint` → `HttpApiGroup` → `HttpApi`. Keep this in a module that both server and client can import. It must contain no handler code.
2. **Implement** each group with `HttpApiBuilder.group(Api, "group", build)`. The result is a `Layer`.
3. **Assemble**: `HttpApiBuilder.layer(Api)` registers routes on the `HttpRouter`. Provide it with the group layers.
4. **Serve**: `HttpRouter.serve(routesLayer)` plus a platform server layer, run with `Layer.launch`. Or use `HttpRouter.toWebHandler` for serverless.
5. **Consume**: `HttpApiClient.make(Api)` gives a fully typed client. `HttpApiTest.groups(Api, [...])` gives an in-memory client for tests.

## Define

```ts
import { Schema } from "effect"

export class UserNotFound extends Schema.TaggedError<UserNotFound>()("UserNotFound", {}, { httpApiStatus: 404 }) {}

export class UsersGroup extends HttpApiGroup.make("users")
  .add(
    HttpApiEndpoint.get("list", "/", {
      query: { search: Schema.optional(Schema.String) },   // struct fields or a schema
      success: Schema.Array(User)
    }),
    HttpApiEndpoint.get("getById", "/:id", {
      params: { id: UserId },          // path params are decoded via the string-tree codec
      success: User,
      error: UserNotFound              // a status from the httpApiStatus annotation
    }),
    HttpApiEndpoint.post("create", "/", {
      payload: CreateUser,             // the JSON body by default
      success: User.pipe(HttpApiSchema.status(201))
    }),
    HttpApiEndpoint.query("search", "/search", {  // HTTP QUERY method: the payload is the request body
      payload: SearchRequest,
      success: Schema.Array(User)
    })
  )
  .middleware(Authorization)            // group-wide; endpoints also have .middleware(...)
  .prefix("/users")
  .annotateMerge(OpenApi.annotations({ title: "Users" }))
{}

export class Api extends HttpApi.make("api").add(UsersGroup).add(SystemGroup) {}
```

- Endpoint constructors: `get`, `post`, `put`, `patch`, `delete`, `head`, `options`, `query`, plus `make(method)`. Options:
  - `params`, `query`, `headers`, `payload`, `success`, `error`. `success` and `error` accept a schema or an array of schemas.
  - `disableCodecs: true` turns off JSON codec derivation.
- GET/HEAD payloads go in the query string. POST/PUT/PATCH/QUERY payloads go in the body.
- `HttpApiGroup.make("system", { topLevel: true })` puts its methods at the root of the generated client (`client.health()`).
- Encodings:
  - `HttpApiSchema.asText({ contentType })`, `asJson`, `asFormUrlEncoded`, `asUint8Array`.
  - `asMultipart()` / `asMultipartStream()`. With `asMultipartStream`, the handler gets `payload: Stream<Multipart.Part>`.
- Status helpers:
  - `HttpApiSchema.status(code)`.
  - `NoContent`, `Created`, `Accepted`, `Empty(code)`.
  - `asNoContent({ decode })` sends errors as bodiless responses.
- Typed response headers: `HttpApiSchema.WithHeaders(schema, { "x-count": Schema.Int })`. The handler returns `HttpApiSchema.withHeaders({ body, headers })`.
- Built-in errors: `HttpApiError.NotFound`, `BadRequest`, `Unauthorized`, `Forbidden`, `Conflict`, `UnprocessableEntity`, `InternalServerError`, ... Each also comes in a `*NoContent` variant.

### Streaming and SSE responses (new in v4)

```ts
HttpApiEndpoint.get("events", "/events", {
  // `data` mode: handlers and clients see the decoded values; framing is JSON-in-SSE
  success: HttpApiSchema.StreamSse({ data: Person, error: StreamError })
  // `events` mode: HttpApiSchema.StreamSse({ events: MyEventSchema }) exposes { id?, event, data }
})
HttpApiEndpoint.get("download", "/file", {
  success: HttpApiSchema.StreamUint8Array({ contentType: "application/pdf" })
})
// handler: () => Effect.succeed(Stream.make(...))
// client:  const stream = yield* client.group.events({})  // Stream<Person, StreamError | ...>
```

Typed stream failures travel as the reserved SSE event `effect/http-api/stream/failure`.

## Implement and serve

```ts
const UsersLive = HttpApiBuilder.group(Api, "users", Effect.fn(function*(handlers) {
  const users = yield* Users   // dependencies are resolved once, when the layer is built
  return handlers.handleAll({
    list: ({ query }) => users.list(query.search).pipe(Effect.orDie),
    getById: ({ params }) => users.findById(params.id),
    create: ({ payload }) => users.create(payload).pipe(Effect.orDie),
    search: ({ payload }) => users.search(payload).pipe(Effect.orDie)
  })
  // or chain: handlers.handle("list", fn).handle(...); handleRaw gives you the raw request and payload
})).pipe(Layer.provide([Users.layer, AuthorizationLive]))

const ApiRoutes = HttpApiBuilder.layer(Api, { openapiPath: "/openapi.json" }).pipe(
  Layer.provide([UsersLive, SystemLive])
)
const AllRoutes = Layer.mergeAll(
  ApiRoutes,
  HttpApiScalar.layer(Api, { path: "/docs" }),     // or HttpApiSwagger.layer(Api, { path })
  HttpStaticServer.layer({ root: "./public", prefix: "/static" }),
  HttpRouter.cors({ allowedOrigins: ["https://app.example.com"] })
)

const Main = HttpRouter.serve(AllRoutes).pipe(
  Layer.provide(NodeHttpServer.layer(createServer, { port: 3000 }))  // node:http createServer
)
Layer.launch(Main).pipe(NodeRuntime.runMain)

// Serverless / fetch-style:
export const { handler, dispose } = HttpRouter.toWebHandler(AllRoutes.pipe(Layer.provide(HttpServer.layerServices)))
```

- Handler input: `{ params, query, payload, headers, request, endpoint, group }`, with only the parts the endpoint declares.
- Handlers fail with the endpoint's declared `error` schemas. Anything else should be `Effect.orDie`'d or mapped.
- Use `Effect.catchReason` / `catchReasons` to map wrapper errors with a `reason` field. See the fixture `server/Users/http.ts`.
- Malformed requests (bad JSON, schema decode failures) produce a 400 `HttpApiSchemaError`. They are not reported as server failures (4.0.1).
- Use `HttpApiMiddleware.layerSchemaErrorTransform(Middleware, (error, { endpoint, group }) => ...)` to reshape schema errors.
- `serve` options:
  - `disableLogger`, `disableListenLog`, `routerConfig`.
  - `middleware`: whole-server middleware such as `HttpMiddleware.compression()` (br/gzip/deflate; zstd is opt-in).
- Graceful shutdown is built in. Platform server options include `gracefulShutdownTimeout` (default 20s) and `disablePreemptiveShutdown`. Interrupting `Layer.launch` (SIGINT via `runMain`) drains in-flight requests.
- Other server layers:
  - `NodeHttpServer.layerConfig(createServer, Config...)`, `NodeHttpServer.layerTest` (a server plus a matching `HttpClient`).
  - `BunHttpServer.layer({ port })`, `DenoHttpServer`.

## Middleware and security

```ts
export class CurrentUser extends Context.Service<CurrentUser, User>()("app/CurrentUser") {}

export class Authorization extends HttpApiMiddleware.Service<Authorization, {
  provides: CurrentUser
  requires: never
}>()("app/Authorization", {
  security: { bearer: HttpApiSecurity.bearer },  // or apiKey({ key, in: "header" | "query" | "cookie" }), basic, http({ scheme })
  error: Unauthorized,
  requiredForClient: true                        // clients must provide HttpApiMiddleware.layerClient(...)
}) {}

// Server implementation (keep it out of the shared api module):
export const AuthorizationLive = Layer.effect(Authorization, Effect.gen(function*() {
  return Authorization.of({
    bearer: Effect.fn(function*(httpEffect, { credential }) {
      if (Redacted.value(credential) !== "dev-token") {
        return yield* new Unauthorized({ message: "invalid token" })
      }
      return yield* Effect.provideService(httpEffect, CurrentUser, devUser)
    })
  })
}))
```

- Middleware without `security` is implemented as `(httpEffect, { endpoint, group }) => Effect<HttpServerResponse, ...>`.
- To set an API-key cookie from a handler: `HttpApiBuilder.securitySetCookie(apiKeySecurity, value)`.
- For plain routes, `HttpRouter.middleware(fn).layer` is route-scoped and `HttpRouter.middleware(fn, { global: true })` is global.
- `HttpMiddleware.logger`, `tracer`, `cors`, `xForwardedHeaders`, and `compression` are available.
- To skip server spans: `HttpMiddleware.layerTracerDisabledForUrls([...])` or the `HttpMiddleware.TracerDisabledWhen` reference.

## Plain routes (no HttpApi)

```ts
const Health = HttpRouter.add("GET", "/health", HttpServerResponse.text("ok"))
const Echo = HttpRouter.add("POST", "/echo", (req) =>
  Effect.map(req.json, (body) => HttpServerResponse.jsonUnsafe(body)))
const Bytes = HttpRouter.add("GET", "/bytes", HttpServerResponse.stream(byteStream, { contentType: "application/octet-stream" }))
const Routes = Layer.mergeAll(Health, Echo, Bytes)  // then HttpRouter.serve(Routes)
```

- Use `HttpRouter.use((router) => ...)` for effectful registration and `HttpRouter.addAll([HttpRouter.route(...)], { prefix })` for groups of routes.
- Inside a handler: `HttpRouter.params`, `HttpRouter.schemaPathParams(S)`, `HttpRouter.schemaParams(S)` (search params plus path params), `HttpServerRequest.schemaBodyJson(S)`.
- `HttpServerResponse.json`, `file` (needs `HttpPlatform`), and template-tag `html` return an `Effect`. `jsonUnsafe`, `text`, `html(string)`, `stream`, `redirect`, and `empty` are synchronous.

## Client

```ts
export class ApiClient extends Context.Service<ApiClient, HttpApiClient.ForApi<typeof Api>>()("app/ApiClient") {
  static readonly layer = Layer.effect(ApiClient, HttpApiClient.make(Api, {
    baseUrl: "http://localhost:3000",
    transformClient: (client) =>
      client.pipe(HttpClient.retryTransient({ schedule: Schedule.exponential(100), times: 3 }))
  })).pipe(
    Layer.provide(HttpApiMiddleware.layerClient(Authorization, ({ next, request }) =>
      next(HttpClientRequest.bearerToken(request, "dev-token")))),
    Layer.provide(FetchHttpClient.layer)
  )
}
// const user = yield* client.users.getById({ params: { id } })
// const [user, res] = yield* client.users.getById({ params: { id }, responseMode: "decoded-and-response" })
```

- `responseMode` can be `"decoded-only"` (the default), `"decoded-and-response"`, or `"response-only"`. It was formerly `withResponse`.
- Related constructors:
  - `HttpApiClient.group(Api, { group, httpClient })` and `HttpApiClient.endpoint(Api, { group, endpoint, httpClient })` build partial clients.
  - `HttpApiClient.urlBuilder(Api, { baseUrl })` builds typed URLs.
  - `HttpApiClient.makeWith(Api, { httpClient, baseUrl })` takes an explicit client.

## Testing

```ts
const makeClient = HttpApiTest.groups(Api, ["users"])  // optional 3rd arg: { baseUrl }
layer(Layer.mergeAll(UsersLiveNoDeps.pipe(Layer.provide(Users.layerMemory), Layer.provideMerge(AuthorizationLive)),
  HttpServer.layerServices))("UsersApi", (it) => {
  it.effect("creates", () => Effect.gen(function*() {
    const client = yield* makeClient
    const user = yield* client.users.create({ payload: { name: "Ada" } })
  }).pipe(Effect.provide(AuthorizationClientGood)))
})
```

`HttpApiTest.groups` runs the real encode → route → handler → decode pipeline in memory. Groups you don't select get handlers that fail if called. Middleware layers that the router resolves must be provided with `provideMerge`.

## v3 → v4

- `@effect/platform` `HttpApi*` → `effect/http-api`. `HttpRouter`, `HttpServer`, `HttpMiddleware` → `effect/http`. `effect/unstable/httpapi` and `effect/unstable/http` no longer exist.
- `HttpApiBuilder.api` + `HttpApiBuilder.serve` + `HttpLayerRouter` → `HttpApiBuilder.layer(Api)` + `HttpRouter.serve`.
- Endpoints take an options object (`{ params, query, payload, success, error }`) instead of `.setPayload`, `.addSuccess`, and similar builder calls.
- Errors declare their status via `{ httpApiStatus }` on `Schema.TaggedError`.
- See `references/migration/api-map/platform.md`.
