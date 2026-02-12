# Sink (Stream Consumption Patterns)

Use this guide when consuming streams into aggregated, validated, or transformed outputs.

## Mental model

- `Stream` produces values; `Sink` consumes them into a result.
- A sink can fail, return leftovers, and compose with other sinks.
- Put final aggregation logic in sinks to keep stream pipelines simple and reusable.

## Patterns

- Use sinks for folds/reductions that should be reusable across stream sources.
- Model parse/validation at consumption boundaries with schema-aware sink stages.
- Keep sinks deterministic and side-effect-light; push IO to surrounding effects.
- Use leftovers when protocol framing requires partial consumption.
- Prefer bounded buffering and backpressure when sink work is expensive.

## Example

```ts
import { Effect, Sink, Stream } from "effect"

const program = Stream.make(1, 2, 3).pipe(
  Stream.run(Sink.sum),
  Effect.map((sum) => ({ sum }))
)
```

## Pitfalls

- Encoding business logic directly in ad-hoc stream callbacks instead of reusable sinks.
- Ignoring leftovers when consuming framed/chunked protocols.
- Combining unbounded streams with expensive sinks without throttling/backpressure.
- Treating sink failures as defects when they are expected parse/domain failures.

## Docs

- `https://effect.website/docs/sink/introduction/`
- `https://effect.website/docs/sink/creating/`
- `https://effect.website/docs/sink/operations/`
- `https://effect.website/docs/sink/leftovers/`
- `https://effect.website/docs/sink/concurrency/`
