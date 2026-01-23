# Observability Examples (Config and Exporters)

Use this guide when you need concrete setup details for logging, metrics, or tracing.

## Logging
- Default logging via `Effect.log` emits at `INFO` level; use `Logger.withMinimumLogLevel` to change the minimum level.
- Log output includes metadata such as timestamp, log level, and fiber id.
- Built-in loggers include `logfmtLogger` and `structuredLogger` (see logging docs for additional options).
- Combine loggers with `Logger.zip` when you want multiple outputs.

## Metrics
- Effect Metrics supports `Counter`, `Gauge`, `Histogram`, `Summary`, and `Frequency`.
- Tag a single metric with `Metric.tagged`, or tag multiple metrics for an effect with `Effect.tagMetrics`.

## Tracing
- Create spans with `Effect.withSpan` and add attributes with `Effect.annotateCurrentSpan`.
- Logs can be recorded as span events when tracing is configured.
- OpenTelemetry integration: install `@effect/opentelemetry` plus the relevant OpenTelemetry SDK packages; `@opentelemetry/api` is required as a peer dependency.

#

## Example

```ts
import * as Effect from "effect/Effect"
import * as OtlpTracer from "@effect/opentelemetry/OtlpTracer"

const program = OtlpTracer.make({
  url: "http://localhost:4318/v1/traces",
  resource: { serviceName: "my-service" }
}).pipe(Effect.scoped)
```
