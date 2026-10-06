# HTTP Client (`effect/http`)

Use this guide when making outbound HTTP requests. Official example:
`references/examples/50_http-client/10_basics.ts`.

All HTTP modules live in `effect` itself (no `@effect/platform` package in v4):

```ts
import { FetchHttpClient, HttpClient, HttpClientError, HttpClientRequest, HttpClientResponse } from "effect/http"
// or per-module: import * as HttpClient from "effect/http/HttpClient"
```

Most `effect/http` APIs are tagged `@stability unstable` (breaking changes allowed in minor releases).

## Mental model

- `HttpClient.HttpClient` is a service. A platform layer provides it:
  - `FetchHttpClient.layer` — any runtime with `fetch` (browser, edge, Node, Bun, Deno). `BunHttpClient` and `DenoHttpClient` simply re-export `FetchHttpClient`.
  - `NodeHttpClient.layerUndici` (undici, owns an `Agent`) or `NodeHttpClient.layerNodeHttp` (`node:http`) from `@effect/platform-node`.
  - `BrowserHttpClient.layerXMLHttpRequest` from `@effect/platform-browser`.
- A client is an immutable value. Middleware such as base URL, auth, status filtering, and retries is applied with `HttpClient.mapRequest`, `HttpClient.filterStatusOk`, and `HttpClient.retryTransient`. This returns a new client. Do it once in a service layer, not at each call site.
- Non-2xx responses **succeed** unless you filter them (`HttpClient.filterStatusOk` on the client, or `HttpClientResponse.filterStatusOk` per response).
- Errors: one `HttpClientError` class (`_tag: "HttpClientError"`) with a `reason` union: `TransportError | EncodeError | InvalidUrlError | StatusCodeError | DecodeError | EmptyBodyError`. `error.request` and `error.response` (when one exists) are getters. Schema decoding failures come through separately as `Schema.SchemaError`.

## Service pattern (canonical)

```ts
import { Context, Effect, flow, Layer, Schedule, Schema } from "effect"
import { FetchHttpClient, HttpClient, HttpClientRequest, HttpClientResponse } from "effect/http"

class Todo extends Schema.Class<Todo>("Todo")({
  id: Schema.Int,
  title: Schema.String,
  completed: Schema.Boolean
}) {}

export class TodoApiError extends Schema.TaggedError<TodoApiError>()("TodoApiError", {
  cause: Schema.Defect()
}) {}

export class TodoApi extends Context.Service<TodoApi, {
  getTodo(id: number): Effect.Effect<Todo, TodoApiError>
  createTodo(todo: Omit<Todo, "id">): Effect.Effect<Todo, TodoApiError>
}>()("app/TodoApi") {
  static readonly layer = Layer.effect(
    TodoApi,
    Effect.gen(function*() {
      const client = (yield* HttpClient.HttpClient).pipe(
        HttpClient.mapRequest(flow(
          HttpClientRequest.prependUrl("https://jsonplaceholder.typicode.com"),
          HttpClientRequest.acceptJson
        )),
        HttpClient.filterStatusOk,
        // Retries TransportError, timeouts and 408/429/500/502/503/504 responses
        HttpClient.retryTransient({ schedule: Schedule.exponential(100), times: 3 })
      )

      const getTodo = Effect.fn("TodoApi.getTodo")(function*(id: number) {
        yield* Effect.annotateCurrentSpan({ id })
        return yield* client.get(`/todos/${id}`, { urlParams: { format: "json" } }).pipe(
          Effect.flatMap(HttpClientResponse.schemaBodyJson(Todo)),
          Effect.mapError((cause) => new TodoApiError({ cause }))
        )
      })

      const createTodo = Effect.fn("TodoApi.createTodo")(function*(todo: Omit<Todo, "id">) {
        return yield* HttpClientRequest.post("/todos").pipe(
          HttpClientRequest.bodyJsonUnsafe(todo),
          client.execute,
          Effect.flatMap(HttpClientResponse.schemaBodyJson(Todo)),
          Effect.mapError((cause) => new TodoApiError({ cause }))
        )
      })

      return TodoApi.of({ getTodo, createTodo })
    })
  ).pipe(Layer.provide(FetchHttpClient.layer))
}
```

## Requests

- Client shorthands: `client.get | post | put | patch | del | head | options | query(url, options?)` and `client.execute(request)`. The `HttpClient.get(url)` accessors and the others use the service from context.
- `HttpClientRequest.Options`: `urlParams`, `hash`, `headers`, `body` (an `HttpBody`), `accept`, `acceptJson`.
- Constructors: `HttpClientRequest.get | post | put | patch | delete | head | options | trace | query`. On the client and on the `HttpClient` module, DELETE is spelled `del`. `query` is the HTTP `QUERY` method, a safe method that carries a body. It is new in v4.
- Combinators: `setHeader(s)`, `updateHeaders`, `removeHeader`, `basicAuth`, `bearerToken`, `accept`, `acceptJson`, `setUrl`, `prependUrl`, `appendUrl`, `updateUrl`, `setUrlParam(s)`, `appendUrlParam(s)`, `setHash`, `removeHash`, `setMethod`.
- Bodies:
  - `bodyJson(value)` returns an `Effect`, because it can fail with `HttpBodyError`. `bodyJsonUnsafe(value)` is synchronous.
  - `HttpClientRequest.schemaBodyJson(Schema)(value)` encodes the body with a schema and returns an `Effect`.
  - Others: `bodyText`, `bodyUint8Array`, `bodyUrlParams`, `bodyFormData`, `bodyFormDataRecord`, `bodyStream`, `bodyFile` (needs `FileSystem`).
- Web interop: `HttpClientRequest.fromWeb` / `toWeb` (also `toWebResult`), and `HttpClientResponse.fromWeb`.

## Responses

- Response fields and effects: `response.status`, `response.headers`, `response.url` (the final URL after redirects), `response.json` (an `Effect<Schema.Json>`), `response.text`, `response.arrayBuffer`, `response.stream`.
- Decoding:
  - `HttpClientResponse.schemaBodyJson(S)` decodes the body.
  - `schemaJson(S)` decodes `{ status, headers, body }` together, which is useful for status-discriminated unions.
  - `schemaNoBody(S)` decodes only status and headers. `schemaHeaders(S)` and `schemaBodyUrlParams(S)` are also available.
- Branching: `HttpClientResponse.matchStatus(response, { 200: ..., "4xx": ..., orElse: ... })`, `filterStatus(predicate)`, `filterStatusOk`.
- Streaming: `HttpClientResponse.stream(client.get(url))` gives a `Stream<Uint8Array>`. For SSE, pipe it through `effect/encoding` `Sse` decoders.

## Error handling

```ts
const status = client.get("/maybe").pipe(
  Effect.map((res) => res.status),
  // HttpClientError carries a `reason`, so use the reason-aware combinators
  Effect.catchReason("HttpClientError", "StatusCodeError", (e) => Effect.succeed(e.response.status))
)
```

You can also catch at the client level with `HttpClient.catchTag`, `catchTags`, `catch`, `filterOrFail`, `filterOrElse`, `tap`, `tapError`, and `tapRequest`.

## Retries, redirects, timeouts

- `HttpClient.retryTransient(options | schedule)`:
  - Options are `{ retryOn?: "errors-only" | "response-only" | "errors-and-responses", while?, schedule?, times? }`. The default is `"errors-and-responses"`. The option was formerly called `mode`, and `"both"` was renamed.
  - Apply it **after** `filterStatusOk` and **before** schema decoding. Only retry idempotent requests.
- `HttpClient.retry(schedule | Effect.Retry.Options)` is a general retry on the client's error channel.
- `HttpClient.followRedirects(max = 10)`. It strips `authorization`, `proxy-authorization`, and `cookie` headers when a redirect goes to another origin.
- Timeouts: use `Effect.timeout` on the request effect. Node undici transport timeouts are deliberately left to Effect.
- `HttpClient.withScope` ties request abort to the current `Scope`. `HttpClient.withCookiesRef(ref)` gives you a cookie jar.

## Rate-limited client (new in v4)

```ts
import { RateLimiter } from "effect/persistence"

const GitHubClient = Effect.gen(function*() {
  const limiter = yield* RateLimiter.RateLimiter
  return (yield* HttpClient.HttpClient).pipe(
    HttpClient.withRateLimiter({
      limiter,
      key: "github",          // or (request) => string for per-host or per-user limits
      limit: 60,
      window: "1 minute",
      algorithm: "fixed-window", // or "token-bucket"
      times: 5                 // max automatic 429 retries (default: unlimited)
    })
  )
})
// Provide: RateLimiter.layer.pipe(Layer.provide(RateLimiter.layerStoreMemory))
// (or RateLimiter.layerStoreRedis for a shared store)
```

`withRateLimiter` reads `RateLimit-*`, `X-RateLimit-*`, and `Retry-After` headers to adapt itself. Override header names with `responseHeaders`. Disable with `disableResponseInspection` or `disableAdaptiveLearning`. Its error type adds `RateLimiter.RateLimiterError`.

## Tracing

The client creates an `http.client <METHOD>` span per request and propagates trace headers by default. Tune this with `Context.Reference`s, provided via `Layer.succeed(ref)(value)` or `Effect.provideService`:

- `HttpClient.TracerDisabledWhen` — `(request) => boolean`
- `HttpClient.TracerPropagationEnabled` — `boolean`
- `HttpClient.TracerHeaderFilter` — `(headerName, "request" | "response") => boolean`. It controls which headers are recorded on spans.
- `HttpClient.SpanNameGenerator` — `(request) => string`

## Platform knobs

- `FetchHttpClient.Fetch` is a `Context.Reference` for a custom `fetch`, for example in tests.
- `FetchHttpClient.RequestInit` is a service with default `RequestInit` values:

  ```ts
  Layer.succeed(FetchHttpClient.RequestInit)({ credentials: "include" })
  ```

- Node:
  - `NodeHttpClient.UndiciOptions` is a reference for undici request options.
  - `NodeHttpClient.layerUndiciNoDispatcher` + `layerDispatcher` / `dispatcherLayerGlobal`.
  - `NodeHttpClient.layerAgentOptions(opts)` configures the `node:http` agent.
  - Undici-specific APIs are `@stability unstable`.

## Pitfalls

- Forgetting a client layer gives you a missing `HttpClient` requirement. Provide it in the service's layer, as above.
- Treating non-2xx as success. Add `filterStatusOk`.
- Calling `bodyJson` as if it were synchronous. It returns an `Effect`. Use `bodyJsonUnsafe` for known-serializable data.
- Retrying POSTs without an idempotency key.
- `HttpApiClient` request option `withResponse` was renamed to `responseMode: "decoded-only" | "decoded-and-response" | "response-only"`. See `http-server.md`.

## v3 → v4

- `@effect/platform/HttpClient*` → `effect/http/HttpClient*`. `effect/unstable/http` paths no longer exist.
- `HttpClientError.RequestError` / `ResponseError` → single `HttpClientError` with a `reason` (see above).
- See `references/migration/api-map/platform.md` and `platform-node.md`.
