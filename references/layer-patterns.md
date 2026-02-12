# Layer Patterns

Use this guide when wiring services and environments.

## Mental model

- Layers build dependency graphs and manage construction.
- Use `Layer.scoped` for resources with lifetimes.
- Provide layers at app boundaries and tests.

## Patterns

- Use `Layer.succeed` for pure values.
- Use `Layer.effect` or `Layer.scoped` for effectful acquisition.
- Combine with `Layer.merge` and provide with `Effect.provide`.

## Walkthrough: service + layer

```ts
import { Effect, Layer } from "effect"

class Greeter extends Effect.Service<Greeter>()("Greeter", {
  sync: () => ({ greet: (name: string) => `hi ${name}` })
}) {}

const Live = Greeter.Default

const program = Greeter.use((g) => g.greet("Ada")).pipe(
  Effect.provide(Live)
)
```

## Pitfalls

- Running effects in constructors instead of layers.
- Creating a fresh layer instance per use (breaks memoization).

## Docs

- `https://effect.website/docs/requirements-management/layers/`
- `https://effect.website/docs/requirements-management/layer-memoization/`
