# Observability Wiring (Layers and Runtime)

Use this guide when wiring logging, metrics, or tracing into an app.

## Patterns

- Provide loggers at the edge.
- Provide tracing layers before running effects.
- Keep exporters in layers.

## Walkthrough: tracing layer

```ts
import { Effect } from "effect"
import * as NodeSdk from "@effect/opentelemetry/NodeSdk"
import { InMemorySpanExporter, SimpleSpanProcessor } from "@opentelemetry/sdk-trace-base"

const TracingLive = NodeSdk.layer(Effect.sync(() => ({
  resource: { serviceName: "app" },
  spanProcessor: [new SimpleSpanProcessor(new InMemorySpanExporter())]
})))

const program = Effect.void.pipe(
  Effect.withSpan("work"),
  Effect.provide(TracingLive)
)
```

## Docs

- `https://effect.website/docs/observability/tracing/`
- `https://effect.website/docs/platform/runtime/`
