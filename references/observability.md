# Observability (Logging, Metrics, Tracing)

Use this guide when instrumenting Effect programs.

For concrete configuration examples (loggers, metrics tagging, tracing exporters), see
`references/observability-examples.md`.

## Logging
- `Effect.log` logs at the default `INFO` level; adjust the minimum level with `Logger.withMinimumLogLevel`.
- Log output includes metadata such as timestamp, log level, and fiber ID.
- Built-in loggers include `stringLogger` (default), `logfmtLogger`, `prettyLogger`, `structuredLogger`, and `jsonLogger`.
- Use log-level helpers (`logDebug`, `logInfo`, `logWarning`, `logError`, `logFatal`) for clarity.
- Use log spans to measure and log task durations; disable logging entirely by setting the minimum level to `None`.

## Metrics
- Effect Metrics supports `Counter`, `Gauge`, `Histogram`, `Summary`, and `Frequency` metric types.
- Tag metrics with key/value labels using `Metric.tagged`; apply common tags to an effect with `Effect.tagMetrics`.

## Tracing
- Use `Effect.withSpan` to create spans around effectful work; traces are composed of spans.
- Use `Effect.annotateCurrentSpan` to attach key/value attributes to the current span.
- Logs can be captured as span events when tracing is configured.
- Export spans via OpenTelemetry using `@effect/opentelemetry` and an SDK (e.g. NodeSdk).

## Example

```ts
import { Effect, Logger, LogLevel } from "effect"

const logger = Logger.make((o) => String(o.message))

const program = Effect.logInfo("hello").pipe(
  Effect.provide(Logger.replace(Logger.defaultLogger, logger)),
  Logger.withMinimumLogLevel(LogLevel.Info)
)
```
