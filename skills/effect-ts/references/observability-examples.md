# Observability Examples (Config and Exporters)

Use this guide for concrete setup details.

## Logging example

```ts
import { Effect, Logger, LogLevel } from "effect"

const program = Effect.logDebug("hello").pipe(
  Logger.withMinimumLogLevel(LogLevel.Debug)
)
```

## Metrics example

```ts
import { Effect, Metric } from "effect"

const counter = Metric.counter("requests")

const program = Effect.gen(function*() {
  yield* Metric.increment(counter)
  return yield* Metric.value(counter)
})
```

## Tracing example (OpenTelemetry)

```ts
import { Effect } from "effect"
import * as OtlpTracer from "@effect/opentelemetry/OtlpTracer"

const program = OtlpTracer.make({
  url: "http://localhost:4318/v1/traces",
  resource: { serviceName: "my-service" }
}).pipe(Effect.scoped)
```

## Docs

- `https://effect.website/docs/observability/logging/`
- `https://effect.website/docs/observability/metrics/`
- `https://effect.website/docs/observability/tracing/`
