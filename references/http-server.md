# HTTP Server (effect/unstable/httpapi)

Use this guide when defining and serving HTTP APIs.

## Mental model

- `HttpApi` defines endpoints, groups, and schemas once.
- `HttpApiBuilder` wires endpoint handlers and produces server layers.
- A platform server layer (Node/Bun) provides the runtime HTTP server.
- `HttpApi` currently lives under `effect/unstable/httpapi` in v4.

## Patterns

- Define endpoints with `HttpApiEndpoint` and group them with `HttpApiGroup`.
- Implement groups with `HttpApiBuilder.group` and assemble with `HttpApiBuilder.layer`.
- Serve with `HttpRouter.serve` and a platform server layer.
- Add Swagger docs via `HttpApiSwagger.layer()`.

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
    HttpApiEndpoint.get("helloWorld", "/", { success: Schema.String })
  )
)

const GreetingsLive = HttpApiBuilder.group(MyApi, "Greetings", (handlers) =>
  handlers.handle("helloWorld", () => Effect.succeed("Hello, World!"))
)

const ApiLive = HttpApiBuilder.layer(MyApi).pipe(
  Layer.provide(GreetingsLive),
  HttpRouter.serve,
  Layer.provide(NodeHttpServer.layer(createServer, { port: 3000 }))
)

Layer.launch(ApiLive).pipe(NodeRuntime.runMain)
```

## Wiring guide

- Provide a platform server layer (`NodeHttpServer.layer`, Bun server layer, etc.).
- Keep request/response schemas on endpoints; handlers return typed values.
- Add `HttpApiSwagger.layer()` when you want auto-generated docs.

## Pitfalls

- Missing the platform server layer in the environment.
- Implementing endpoints without wiring the group into `HttpApiBuilder.layer`.
- Running effects inside handlers that should be provided via layers.
