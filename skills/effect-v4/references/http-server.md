# HTTP Server (@effect/platform)

Use this guide when defining and serving HTTP APIs.

## Mental model

- `HttpApi` defines endpoints, groups, and schemas once.
- `HttpApiBuilder.layer(Api)` wires endpoint handlers and produces server layers.
- A platform server layer (Node/Bun) provides the runtime HTTP server.

## Patterns

- Define endpoints with `HttpApiEndpoint` and group them with `HttpApiGroup`.
- Implement groups with `HttpApiBuilder.group` and assemble with `HttpApiBuilder.layer(Api)`.
- Serve by piping through `HttpRouter.serve` and providing a platform server layer.
- Prefer the built-in graceful shutdown support instead of rolling your own signal/finalizer wiring.
- Use `HttpApiMiddleware.layerSchemaErrorTransform` when schema-decoding errors need to be mapped into your API error model.
- Add Scalar or Swagger docs via `HttpApiScalar.layer(Api)` / `HttpApiSwagger.layer(Api)`.
- Use `HttpApiTest` for endpoint tests; `HttpApiTest.groups` accepts a `baseUrl` override.
- Use `HttpApiSecurity.http` for custom HTTP security schemes and `HttpApiBuilder.securitySetCookie` for security cookies.
- Use streaming request/response schemas for binary or SSE-style endpoints instead of bypassing `HttpApi`.

## Walkthrough: Hello World server

```ts
import {
  HttpApi,
  HttpApiBuilder,
  HttpApiEndpoint,
  HttpApiGroup
} from "effect/unstable/httpapi"
import { HttpRouter } from "effect/unstable/http"
import { NodeHttpServer, NodeRuntime } from "@effect/platform-node"
import { Effect, Layer, Schema } from "effect"
import { createServer } from "node:http"

const MyApi = HttpApi.make("MyApi").add(
  HttpApiGroup.make("Greetings").add(
    HttpApiEndpoint.get("hello-world", "/", {
      success: Schema.String
    })
  )
)

const GreetingsLive = HttpApiBuilder.group(MyApi, "Greetings", (handlers) =>
  handlers.handle("hello-world", () => Effect.succeed("Hello, World!"))
)

const MyApiLive = HttpApiBuilder.layer(MyApi).pipe(Layer.provide(GreetingsLive))

const ServerLive = MyApiLive.pipe(
  HttpRouter.serve,
  Layer.provide(NodeHttpServer.layer(createServer, { port: 3000 }))
)

Layer.launch(ServerLive).pipe(NodeRuntime.runMain)
```

## Wiring guide

- Provide a platform server layer (`NodeHttpServer.layer`, Bun server layer, etc.).
- Keep request/response schemas on endpoints; handlers return typed values.
- Add `HttpApiScalar.layer(Api)` or `HttpApiSwagger.layer(Api)` when you want auto-generated docs.

## Pitfalls

- Missing the platform server layer in the environment.
- Implementing endpoints without wiring the group into `HttpApiBuilder.layer(Api)`.
- Running effects inside handlers that should be provided via layers.
- Expecting malformed JSON request bodies to reach handlers; current HttpApi returns 400 for malformed JSON.
- Letting handler errors trigger security middleware fallback; current behavior keeps handler failures separate from security fallback.

## Docs

- `https://effect.website/docs/platform/introduction/`
- `https://effect.website/docs/platform/runtime/`
