# Observability Wiring (Exporters, Env Config, Runtime)

Use this guide to ship logs, traces and metrics out of an Effect 4.0.x app.
Instrumentation APIs: `references/observability.md`. Recipes:
`references/observability-examples.md`. Official example:
`references/examples/08_observability/20_otlp-tracing.ts`.

## Which exporter?

| Situation | Use |
| --- | --- |
| New project, OTLP collector / vendor endpoint | `effect/observability` `Otlp*` modules (no OpenTelemetry SDK deps) |
| Prometheus pull scraping | `PrometheusMetrics.layerHttp` (see examples file) |
| Existing OpenTelemetry SDK setup, auto-instrumentations, OTel processors | `@effect/opentelemetry` `NodeSdk` / `WebSdk` |

All `effect/observability` modules are tagged `@stability unstable` (may change
in minor releases).

## OTLP: all three signals at once

```ts
import { NodeRuntime } from "@effect/platform-node"
import { Effect, Layer } from "effect"
import { FetchHttpClient } from "effect/http"
import { Otlp } from "effect/observability"

// POSTs to <baseUrl>/v1/logs, /v1/metrics, /v1/traces
export const Observability = Otlp.layerJson({        // or Otlp.layerProtobuf
  baseUrl: "http://localhost:4318",
  resource: { serviceName: "checkout-api", serviceVersion: "1.0.0",
    attributes: { "deployment.environment": "staging" } },
  headers: { authorization: "Bearer ..." }
  // optional: maxBatchSize, tracerExportInterval, loggerExportInterval,
  // metricsExportInterval, metricsTemporality ("cumulative" | "delta"),
  // loggerExcludeLogSpans, loggerMergeWithExisting (default true), shutdownTimeout
}).pipe(Layer.provide(FetchHttpClient.layer))

declare const AppLayer: Layer.Layer<never>

// Provide observability LAST so layer-construction spans/logs are exported too
Layer.launch(AppLayer.pipe(Layer.provide(Observability))).pipe(NodeRuntime.runMain)
```

`Otlp.layer(options)` is the same but also requires an
`OtlpSerialization.OtlpSerialization` (`OtlpSerialization.layerJson` or
`OtlpSerialization.layerProtobuf`). Every OTLP layer requires an
`HttpClient` (`FetchHttpClient.layer`, or a platform client such as
`NodeHttpClient.layerUndici`).

## OTLP: per signal

```ts
import { Layer } from "effect"
import { FetchHttpClient } from "effect/http"
import { OtlpLogger, OtlpMetrics, OtlpSerialization, OtlpTracer } from "effect/observability"

const resource = { serviceName: "checkout-api" }

export const Observability = Layer.mergeAll(
  OtlpTracer.layer({ url: "http://localhost:4318/v1/traces", resource }),   // provides Tracer
  OtlpLogger.layer({ url: "http://localhost:4318/v1/logs", resource }),     // adds a logger (merge by default)
  OtlpMetrics.layer({ url: "http://localhost:4318/v1/metrics", resource, temporality: "delta" })
).pipe(
  Layer.provide(OtlpSerialization.layerJson),
  Layer.provide(FetchHttpClient.layer)
)
```

Per-signal options: `url`, `resource`, `headers`, `exportInterval`,
`shutdownTimeout` (default 3s); tracer/logger also `maxBatchSize`; tracer
`context`; logger `excludeLogSpans`, `mergeWithExisting`; metrics `temporality`.

## OTLP from standard `OTEL_*` environment variables

```ts
import { Layer } from "effect"
import { FetchHttpClient } from "effect/http"
import { Otlp, OtlpSerialization } from "effect/observability"

export const Observability = Otlp.layerFromConfig({ resource: { serviceVersion: "1.0.0" } }).pipe(
  Layer.provide(OtlpSerialization.layerProtobuf),
  Layer.provide(FetchHttpClient.layer)
)
// Also per signal: OtlpTracer.layerFromConfig / OtlpLogger.layerFromConfig / OtlpMetrics.layerFromConfig
```

Read through `Config` (so any `ConfigProvider` works, default = env):

| Variable | Effect |
| --- | --- |
| `OTEL_SDK_DISABLED=true` | install nothing |
| `OTEL_TRACES_EXPORTER`, `OTEL_LOGS_EXPORTER`, `OTEL_METRICS_EXPORTER` | comma list; **must contain `otlp`** or that signal is not exported |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | base URL; `/v1/<signal>` is appended |
| `OTEL_EXPORTER_OTLP_<SIGNAL>_ENDPOINT` | full per-signal URL (wins) |
| `OTEL_EXPORTER_OTLP_HEADERS`, `..._<SIGNAL>_HEADERS` | `k=v,k2=v2`, percent-decoded |
| `OTEL_SERVICE_NAME`, `OTEL_SERVICE_VERSION`, `OTEL_RESOURCE_ATTRIBUTES` | resource |
| `OTEL_EXPORTER_OTLP_TIMEOUT`, `..._<SIGNAL>_TIMEOUT` | shutdown timeout (ms) |
| `OTEL_BSP_SCHEDULE_DELAY`, `OTEL_BSP_MAX_EXPORT_BATCH_SIZE`, `OTEL_BSP_EXPORT_TIMEOUT` | traces batching |
| `OTEL_BLRP_SCHEDULE_DELAY`, `OTEL_BLRP_MAX_EXPORT_BATCH_SIZE`, `OTEL_BLRP_EXPORT_TIMEOUT` | logs batching |
| `OTEL_METRIC_EXPORT_INTERVAL`, `OTEL_METRIC_EXPORT_TIMEOUT`, `OTEL_EXPORTER_OTLP_METRICS_TEMPORALITY_PREFERENCE` | metrics |

`<SIGNAL>` is `TRACES`, `LOGS` or `METRICS`. Explicit `resource` options win
over attributes, which win over env.

## Gotchas

- A service name is required: pass `resource.serviceName` or set
  `OTEL_SERVICE_NAME`, otherwise building the layer dies.
- Per-signal layers output `OtlpExporter.Flusher`; call it to drain buffers
  before short-lived processes exit (exporters also flush on scope close,
  bounded by `shutdownTimeout`):

  ```ts
  import { Effect } from "effect"
  import { OtlpExporter } from "effect/observability"

  const flushNow = Effect.gen(function*() {
    const flusher = yield* OtlpExporter.Flusher
    yield* flusher.flush.pipe(Effect.timeoutOption("2 seconds"))
  })
  ```

- Telemetry is only exported while the layer's scope is open: use
  `Layer.launch` + `NodeRuntime.runMain`, `ManagedRuntime.make(layer)` (dispose
  it on shutdown), or `Effect.provide(Observability)` around the whole program.
- `OtlpLogger.layer` *merges* with existing loggers by default; set
  `mergeWithExisting: false` to make OTLP the only logger.
- Repeated export failures temporarily disable an exporter (60s) rather than
  failing your app.

## Existing OpenTelemetry SDK: `@effect/opentelemetry`

Install `@effect/opentelemetry` (same version as `effect`) plus the
`@opentelemetry/*` packages you use (`sdk-trace-base`, `sdk-trace-node`,
`sdk-metrics`, `sdk-logs`, `resources`, `api`, ...).

```ts
import { NodeSdk } from "@effect/opentelemetry"
import { OTLPTraceExporter } from "@opentelemetry/exporter-trace-otlp-http"
import { BatchSpanProcessor } from "@opentelemetry/sdk-trace-base"
import { Effect } from "effect"

export const TracingLive = NodeSdk.layer(() => ({
  resource: { serviceName: "checkout-api" },
  spanProcessor: new BatchSpanProcessor(new OTLPTraceExporter())
  // also: tracerConfig, metricReader, metricTemporality, logRecordProcessor,
  // loggerProviderConfig, loggerMergeWithExisting, shutdownTimeout
}))
// NodeSdk.layer also accepts an Effect<Configuration> (e.g. to read Config)

const program = Effect.log("hi").pipe(Effect.withSpan("work"), Effect.provide(TracingLive))
```

Only signals with a processor/reader are enabled. Other modules:
`OtelTracer` (`layerGlobal` to reuse a globally registered provider,
`currentOtelSpan`, `withSpanContext`, `makeExternalSpan`), `OtelLogger.layer`,
`OtelMetrics.layer`, `Resource.layer` / `Resource.layerFromEnv`, `WebSdk.layer`
for browsers. Register Node auto-instrumentations before importing the modules
they patch.

## v3 -> v4

- `@effect/opentelemetry/Otlp`, `OtlpTracer`, `OtlpLogger`, `OtlpMetrics` moved
  to `effect/observability`; `replaceLogger` -> `mergeWithExisting` /
  `loggerMergeWithExisting`.
- `@effect/opentelemetry` `Logger`/`Metrics`/`Tracer` modules are now
  `OtelLogger`/`OtelMetrics`/`OtelTracer`.
- See `references/migration/api-map/opentelemetry.md`.
