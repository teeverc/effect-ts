# Observability (Logging, Tracing, Metrics)

Use this guide when instrumenting Effect 4.0.x code. Logging, tracing and
metrics are built into `effect` itself; exporters live in `effect/observability`
(see `references/observability-wiring.md`). Recipes:
`references/observability-examples.md`. Official examples:
`references/examples/08_observability/10_logging.ts`,
`references/examples/08_observability/20_otlp-tracing.ts`.

## Mental model

- **Logs** go to the set of loggers in `References.CurrentLoggers`
  (default: `Logger.defaultLogger` + `Logger.tracerLogger`, which turns logs into
  span events). `Logger.layer([...])` *replaces* that set unless
  `{ mergeWithExisting: true }`.
- **Log level filtering** is a context reference: `References.MinimumLogLevel`
  (default `"Info"`, so `logDebug`/`logTrace` are dropped by default).
  Levels are string literals: `"All" | "Fatal" | "Error" | "Warn" | "Info" | "Debug" | "Trace" | "None"`.
- **Spans** come from the `Tracer.Tracer` reference. Without an exporter layer
  the native in-process tracer still creates spans (cheap, not exported).
- **Metrics** are plain values (`Metric.counter(...)`) registered lazily in the
  `Metric.MetricRegistry` reference on first update. They are no longer callable
  or pipeable onto effects - use `Metric.update` or `Effect.track*`.

## Logging

```ts
import { Effect, Layer, References } from "effect"

const program = Effect.gen(function*() {
  yield* Effect.log("default level (Info)")          // variadic: any values
  yield* Effect.logDebug("dropped unless MinimumLogLevel <= Debug")
  yield* Effect.logInfo("validating cart", { cartId: "c_1" })
  yield* Effect.logWarning("inventory low")
  yield* Effect.logError("payment timeout")
  yield* Effect.logFatal("unrecoverable")
  yield* Effect.logWithLevel("Trace")("explicit level") // curried
}).pipe(
  Effect.annotateLogs({ service: "checkout-api" }), // or annotateLogs("key", value)
  Effect.withLogSpan("checkout")                    // adds checkout=<N>ms to each line
)

// Lower the threshold for one effect...
const verbose = program.pipe(Effect.provideService(References.MinimumLogLevel, "Debug"))
// ...or app-wide via a layer
const DebugLogs = Layer.succeed(References.MinimumLogLevel, "Debug")
```

- `Effect.annotateLogsScoped(...)` annotates until the current scope closes.
- `Effect.withLogger(logger)` adds a logger for one effect only.
- Passing a `Cause` as a log argument attaches it as the entry's cause.
- `LogLevel` module: `LogLevel.isGreaterThanOrEqualTo`, `LogLevel.Order`,
  `LogLevel.values` for comparisons; there is no `LogLevel.Debug` constant.

## Tracing

```ts
import { Context, Effect, Layer } from "effect"

// Effect.fn("name") wraps every call in a span named "name" (preferred)
const chargeCard = Effect.fn("Payments.chargeCard")(function*(orderId: string) {
  yield* Effect.annotateCurrentSpan("order.id", orderId)  // or ({ ... })
  yield* Effect.sleep("50 millis")
})

const persist = Effect.sleep("20 millis").pipe(
  Effect.withSpan("checkout.persist", { attributes: { table: "orders" }, kind: "client" })
)

const checkout = Effect.gen(function*() {
  yield* chargeCard("ord_1")
  yield* persist
  const span = yield* Effect.currentSpan          // fails with NoSuchElementError if none
  yield* Effect.log("trace", span.traceId)
}).pipe(
  Effect.withSpan("checkout"),
  Effect.annotateSpans({ "checkout.provider": "acme" }) // all child spans
)

// Layers can be traced too: spans cover layer construction
const AppLayer = Layer.empty.pipe(Layer.withSpan("app.init"))
```

Span options (`Tracer.SpanOptions`): `attributes`, `links`, `parent`, `root`,
`annotations`, `kind`, `sampled`, `level`, `captureStackTrace`.
Other tools: `Effect.withSpanScoped`, `Effect.makeSpan`/`makeSpanScoped`,
`Effect.useSpan`, `Effect.withParentSpan(span)`, `Effect.linkSpans(span, attrs?)`,
`Effect.currentParentSpan`, `Effect.withTracerEnabled(false)`,
`Effect.withTracer(tracer)`, `Tracer.externalSpan({ traceId, spanId })` for
propagated parents. `Effect.fnUntraced` creates no span - use it in hot or
library paths.

Trace-level filtering: spans have a `level` (default `"Info"`);
`Tracer.MinimumTraceLevel` drops spans below a threshold.

## Metrics

```ts
import { Effect, Metric } from "effect"

const requests = Metric.counter("http_requests_total", { description: "Requests" })
const inFlight = Metric.gauge("http_in_flight")
const latency = Metric.timer("http_latency")                       // Histogram<Duration>, ms buckets
const sizes = Metric.histogram("body_bytes", {
  boundaries: Metric.exponentialBoundaries({ start: 64, factor: 2, count: 12 })
})
const statuses = Metric.frequency("http_status")

const handle = Effect.gen(function*() {
  yield* Metric.update(requests, 1)                        // counters add
  yield* Metric.modify(inFlight, 1)                        // gauge delta (update sets absolute)
  yield* Metric.update(statuses, "200")
  yield* Metric.update(sizes, 1024)
}).pipe(
  Effect.trackDuration(latency),                           // record elapsed time
  Effect.ensuring(Metric.modify(inFlight, -1))
)

// Per-call attributes (labels)
const tagged = Metric.withAttributes(requests, { route: "/checkout" })
```

- `Effect.track(metric, exit => input)`, `Effect.trackSuccesses`,
  `Effect.trackErrors`, `Effect.trackDefects`, `Effect.trackDuration`.
- Read: `Metric.value(metric)`, `Metric.snapshot`, `Metric.dump`.
- Fiber runtime metrics are opt-in: `Metric.enableRuntimeMetricsLayer`.
- Isolate in tests: `Effect.provideService(Metric.MetricRegistry, new Map())`.

## Pitfalls

- `Logger.layer([myLogger])` drops the default *and* `tracerLogger`; include
  `Logger.tracerLogger` or pass `{ mergeWithExisting: true }` if you still want
  logs as span events.
- No `Metric.increment`, no `Logger.withMinimumLogLevel`, no
  `Logger.minimumLogLevel`, no `LogLevel.Debug` objects in v4 (see below).
- Exporter layers must be provided *outermost* (last) so every span, including
  layer-construction spans, is exported.
- Log message objects are serialized by structured/JSON loggers - wrap secrets
  in `Redacted`.
- Spans only nest under a parent if the child runs inside the parent's effect;
  use `Effect.withParentSpan` / `{ parent }` / `{ root: true }` to override.

## v3 -> v4

- `Logger.add/replace/minimumLogLevel` -> `Logger.layer([...], { mergeWithExisting })`
  and `Layer.succeed(References.MinimumLogLevel, "Debug")`.
- `LogLevel.Debug` (object) -> `"Debug"` (string literal).
- `Metric.increment(c)` -> `Metric.update(c, 1)`; metrics are not callable.
- `FiberRef.currentLogAnnotations` etc. -> `References.CurrentLogAnnotations`,
  `References.CurrentLogSpans`, `References.TracerEnabled`, ...
- `@effect/opentelemetry/Otlp*` -> `effect/observability`.
- Details: `references/migration/api-map/effect-Logger.md`,
  `references/migration/api-map/effect-Metric.md`,
  `references/migration/api-map/effect-Tracer.md`,
  `references/migration/api-map/opentelemetry.md`, `references/migration/fiberref.md`.
