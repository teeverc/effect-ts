# Observability Wiring (Layers and Runtime)

Use this guide when you need to wire logging, metrics, or tracing into an app.

- Configure logging with `Logger.withMinimumLogLevel` and provide a logger layer to the app (set level to `None` to disable).
- Use `Metric` constructors and `Metric.tagged` in effects; export metrics via your chosen backend.
- Create spans with `Effect.withSpan` / `Effect.annotateCurrentSpan` and provide a tracing layer.
- Provide observability layers at the program edge using `Effect.provide` or `Layer.provide`.
- Keep instrumentation close to domain logic; keep exporters and sinks in layers.
