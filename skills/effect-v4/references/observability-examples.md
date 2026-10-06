# Observability Recipes (Loggers, Metrics, Spans)

Concrete, copy-ready patterns for Effect 4.0.x. Concepts:
`references/observability.md`. Exporters/runtime wiring:
`references/observability-wiring.md`. Full official example:
`references/examples/08_observability/10_logging.ts`.

## Choosing loggers

```ts
import { NodeFileSystem } from "@effect/platform-node"
import { Config, Effect, Layer, Logger, References } from "effect"

// Built-ins (values unless noted):
//   Logger.defaultLogger, Logger.consolePretty(opts?) (function),
//   Logger.consoleJson, Logger.consoleLogFmt, Logger.consoleStructured,
//   Logger.tracerLogger (logs -> span events)
// Formatters returning output (wrap with Logger.withConsoleLog / withLeveledConsole):
//   Logger.formatSimple, Logger.formatLogFmt, Logger.formatStructured, Logger.formatJson

export const JsonLogs = Logger.layer([Logger.consoleJson, Logger.tracerLogger])
export const PrettyLogs = Logger.layer([Logger.consolePretty({ colors: "auto" })])

// Append to file (scoped; needs FileSystem). Options: flag, mode, batchWindow
export const FileLogs = Logger.layer(
  [Logger.toFile(Logger.formatSimple, "app.log")],
  { mergeWithExisting: true } // keep console logging too
).pipe(Layer.provide(NodeFileSystem.layer))

// Switch by environment
export const LoggerLayer = Layer.unwrap(Effect.gen(function*() {
  const env = yield* Config.String("NODE_ENV").pipe(Config.withDefault("development"))
  return env === "production"
    ? JsonLogs.pipe(Layer.provideMerge(Layer.succeed(References.MinimumLogLevel, "Info")))
    : PrettyLogs.pipe(Layer.provideMerge(Layer.succeed(References.MinimumLogLevel, "Debug")))
}))
```

## Custom and batched loggers

```ts
import { Effect, Logger } from "effect"

// Logger.make receives { message, logLevel, cause, fiber, date }.
// `message` is the array of arguments passed to Effect.log*.
export const lineLogger = Logger.make(({ date, logLevel, message }) =>
  `${date.toISOString()} ${logLevel} ${(message as ReadonlyArray<unknown>).map(String).join(" ")}`
)
export const consoleLineLogger = Logger.withConsoleLog(lineLogger)

// Batch structured entries and ship them somewhere (scoped effect -> Logger)
export const shippingLogger = Logger.batched(Logger.formatStructured, {
  window: "1 second",
  flush: Effect.fn(function*(batch) {
    yield* Effect.sync(() => console.log(`shipping ${batch.length} entries`))
  })
})
export const ShippingLogs = Logger.layer([shippingLogger], { mergeWithExisting: true })

// Transform output of an existing logger
export const upperJson = Logger.map(Logger.formatJson, (s) => s.toUpperCase())
```

## Capturing logs in tests

```ts
import { assert, it } from "@effect/vitest"
import { Effect, Logger, References } from "effect"

it.effect("logs a warning", () =>
  Effect.gen(function*() {
    const seen: Array<string> = []
    const capture = Logger.make(({ logLevel, message }) => {
      seen.push(`${logLevel}:${(message as ReadonlyArray<unknown>).join(" ")}`)
    })
    yield* Effect.logWarning("low stock").pipe(
      Effect.provide(Logger.layer([capture])),
      Effect.provideService(References.MinimumLogLevel, "All")
    )
    assert.deepStrictEqual(seen, ["Warn:low stock"])
  }))
```

## Metric recipes

```ts
import { Effect, Metric } from "effect"

const jobs = Metric.counter("jobs_total", { description: "Processed jobs" })
const jobErrors = Metric.counter("job_errors_total")
const jobLatency = Metric.timer("job_duration")      // ms buckets, attribute time_unit=milliseconds
const queueDepth = Metric.gauge("queue_depth")
const payload = Metric.summary("payload_bytes", {
  maxAge: "1 minute", maxSize: 1000, quantiles: [0.5, 0.9, 0.99]
})

const processJob = Effect.fn("processJob")(function*(id: string, bytes: number) {
  yield* Metric.update(payload, bytes)
  yield* Effect.annotateCurrentSpan("job.id", id)
},
  Effect.trackDuration(jobLatency),
  Effect.trackErrors(jobErrors, () => 1),               // map error -> counter input
  Effect.trackSuccesses(Metric.withAttributes(jobs, { outcome: "ok" }), () => 1)
)
// Effect.track(metric, (exit) => ...) sees the full Exit (e.g. Exit.isSuccess(exit))

// Absolute vs delta for gauges
const setDepth = (n: number) => Metric.update(queueDepth, n)
const bumpDepth = Metric.modify(queueDepth, 1)

// Read back (tests / debug)
const inspect = Effect.gen(function*() {
  const state = yield* Metric.value(jobs)    // CounterState: { count }
  const text = yield* Metric.dump            // human-readable table of all metrics
  return { count: state.count, text }
}).pipe(Effect.provideService(Metric.MetricRegistry, new Map())) // isolated registry
```

Bucket helpers: `Metric.linearBoundaries({ start, width, count })`,
`Metric.exponentialBoundaries({ start, factor, count })`.
Counters accept `{ bigint: true }` and `{ incremental: true }` (increment-only).

## Prometheus scrape endpoint

```ts
import { Layer } from "effect"
import { HttpRouter } from "effect/http"
import { PrometheusMetrics } from "effect/observability"

// Adds GET /metrics (text/plain; version=0.0.4) to the current HttpRouter
const MetricsRoute = PrometheusMetrics.layerHttp({ prefix: "myapp" }) // also: path, metricNameMapper

// Compose with your other routes and serve:
// HttpRouter.serve(Layer.mergeAll(ApiRoutes, MetricsRoute)).pipe(Layer.provide(NodeHttpServer.layer(...)))
declare const ApiRoutes: Layer.Layer<never, never, HttpRouter.HttpRouter>
export const Routes = Layer.mergeAll(ApiRoutes, MetricsRoute)

// Or format on demand: PrometheusMetrics.format({ prefix: "myapp" }) : Effect<string>
```

## Span recipes

```ts
import { Effect } from "effect"
import { HttpClient, HttpMiddleware } from "effect/http"

// Manual span with access to the Span object
const withHandle = Effect.useSpan("batch.import", { kind: "consumer" }, (span) =>
  Effect.sync(() => span.attribute("batch.size", 42))
)

// Link a background span to the current request span
const linked = Effect.gen(function*() {
  const origin = yield* Effect.currentSpan
  yield* Effect.log("async work").pipe(
    Effect.withSpan("background", { root: true, links: [{ span: origin, attributes: { "link.type": "follows-from" } }] })
  )
})

// HTTP: skip tracing health checks (server) / control client propagation
const NoHealthSpans = HttpMiddleware.layerTracerDisabledForUrls(["/health", "/metrics"])
const noPropagation = Effect.provideService(HttpClient.TracerPropagationEnabled, false)
```

Also: `HttpMiddleware.TracerDisabledWhen`, `HttpMiddleware.SpanNameGenerator`,
`HttpClient.TracerDisabledWhen`, `HttpClient.SpanNameGenerator` are
`Context.Reference`s you can override with `Effect.provideService` or
`Layer.succeed`.
