# Observability Examples (Config and Exporters)

Use this guide for concrete setup details.

## Logging example

```ts
import * as Effect from "effect/Effect"
import * as Logger from "effect/Logger"
import * as References from "effect/References"

const program = Effect.logInfo("hello").pipe(
  Effect.provide(Logger.layer([Logger.consolePretty()])),
  Effect.provideService(References.MinimumLogLevel, "Info")
)
```

## Metrics example

```ts
import * as Effect from "effect/Effect"
import * as Metric from "effect/Metric"

const counter = Metric.counter("requests")

const program = Effect.succeed(1).pipe(
  Metric.update(counter, 1)
)
```

## Tracing example (OpenTelemetry)

```ts
import * as Effect from "effect/Effect"
import * as OtlpTracer from "effect/unstable/observability/OtlpTracer"
import * as OtlpSerialization from "effect/unstable/observability/OtlpSerialization"
import * as FetchHttpClient from "effect/unstable/http/FetchHttpClient"

const program = OtlpTracer.make({
  url: "http://localhost:4318/v1/traces",
  resource: { serviceName: "my-service" }
}).pipe(
  Effect.scoped,
  Effect.provide(FetchHttpClient.layer),
  Effect.provide(OtlpSerialization.layerJson)
)
```
