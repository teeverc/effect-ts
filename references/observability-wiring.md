# Observability Wiring (Layers and Runtime)

Use this guide when you need to wire logging, metrics, or tracing into an app.

- Configure logging with `Logger.withMinimumLogLevel` and provide a logger layer to the app (set level to `None` to disable).
- Use `Metric` constructors and `Metric.tagged` in effects; export metrics via your chosen backend.
- Create spans with `Effect.withSpan` / `Effect.annotateCurrentSpan` and provide a tracing layer.
- Provide observability layers at the program edge using `Effect.provide` or `Layer.provide`.
- Keep instrumentation close to domain logic; keep exporters and sinks in layers.

## Example

```ts
import * as NodeSdk from "@effect/opentelemetry/NodeSdk"
import { InMemorySpanExporter, SimpleSpanProcessor } from "@opentelemetry/sdk-trace-base"
import * as Effect from "effect/Effect"

const TracingLive = NodeSdk.layer(Effect.sync(() => ({
  resource: { serviceName: "test" },
  spanProcessor: [new SimpleSpanProcessor(new InMemorySpanExporter())]
})))

const program = Effect.withSpan("work")(Effect.void).pipe(
  Effect.provide(TracingLive)
)
```
