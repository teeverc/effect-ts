# v3 → v4 API map: @effect/opentelemetry (11 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/opentelemetry/Logger`

- `Logger.layerLoggerAdd` -> `OtelLogger.layer({ mergeWithExisting: true })`: The Logger module was renamed to OtelLogger; logger installation is now one configurable layer, with true preserving the v3 additive behavior.

- `Logger.layerLoggerReplace` -> `OtelLogger.layer({ mergeWithExisting: false })`: The Logger module was renamed to OtelLogger; logger installation is now one configurable layer, with false replacing existing loggers.

- `Logger.make` -> `OtelLogger.make`: The constructor remains in the renamed OtelLogger module.

## `@effect/opentelemetry/Metrics`

- `Metrics.layer` -> `OtelMetrics.layer`: The Metrics module was renamed to OtelMetrics; the layer remains and now also accepts an optional temporality setting.

## `@effect/opentelemetry/NodeSdk`

- `NodeSdk.Configuration` -> `NodeSdk.Configuration`: The configuration interface remains; v4 adds metricTemporality and loggerMergeWithExisting options.

## `@effect/opentelemetry/Otlp`

- `Otlp.layer` -> `Otlp.layer`: Moved to effect/observability/Otlp; replaceLogger was replaced by loggerMergeWithExisting, and metricsTemporality is now configurable.

## `@effect/opentelemetry/OtlpLogger`

- `OtlpLogger.layer` -> `OtlpLogger.layer`: Moved to effect/observability/OtlpLogger; use mergeWithExisting instead of passing replaceLogger.

- `OtlpLogger.make` -> `OtlpLogger.make`: The constructor remains in the module moved to effect/observability/OtlpLogger.

## `@effect/opentelemetry/OtlpMetrics`

- `OtlpMetrics.layer` -> `OtlpMetrics.layer`: Moved to effect/observability/OtlpMetrics; the layer now also accepts optional cumulative or delta temporality.

- `OtlpMetrics.make` -> `OtlpMetrics.make`: Moved to effect/observability/OtlpMetrics; the constructor now also accepts optional cumulative or delta temporality.

## `@effect/opentelemetry/OtlpResource`

- `OtlpResource.unsafeServiceName` -> `OtlpResource.serviceNameUnsafe`: Moved to effect/observability/OtlpResource and renamed to follow the v4 unsafe-suffix convention.

## `@effect/opentelemetry/OtlpTracer`

- `OtlpTracer.layer` -> `OtlpTracer.layer`: The layer remains in the module moved to effect/observability/OtlpTracer.

- `OtlpTracer.make` -> `OtlpTracer.make`: The constructor remains in the module moved to effect/observability/OtlpTracer.

## `@effect/opentelemetry/Resource`

- `Resource.Resource` -> `Resource.Resource`: The service remains in @effect/opentelemetry/Resource but is now a Context.Service class rather than a separate Tag interface and value.

## `@effect/opentelemetry/Tracer`

- `Tracer.OtelTraceFlags` -> `OtelTracer.OtelTraceFlags`: The service moved with the module and is now declared as a Context.Service class.

- `Tracer.OtelTraceState` -> `OtelTracer.OtelTraceState`: The service moved with the module and is now declared as a Context.Service class.

- `Tracer.OtelTracer` -> `OtelTracer.OtelTracer`: The service moved with the renamed module and is now declared as a Context.Service class.

- `Tracer.OtelTracerProvider` -> `OtelTracer.OtelTracerProvider`: The service moved with the renamed module and is now declared as a Context.Service class.

- `Tracer.layer` -> `OtelTracer.layer`: The Tracer module was renamed to OtelTracer; this still creates an OpenTelemetry tracer and installs it as Effect's tracer.

- `Tracer.layerTracer` -> `OtelTracer.layerTracer`: The Tracer module was renamed to OtelTracer; this layer still creates only the OpenTelemetry tracer service.

- `Tracer.make` -> `OtelTracer.make`: The constructor remains in the renamed OtelTracer module.

## `@effect/opentelemetry/WebSdk`

- `WebSdk.Configuration` -> `WebSdk.Configuration`: The configuration interface remains; v4 adds metricTemporality and loggerMergeWithExisting options.
