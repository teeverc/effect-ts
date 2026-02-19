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
import { Effect, Layer, ServiceMap } from "effect"

class Greeter extends ServiceMap.Service<Greeter>()("Greeter", {
  make: Effect.succeed({ greet: (name: string) => `hi ${name}` })
}) {
  static layer = Layer.effect(this, this.make)
}

const program = Effect.gen(function* () {
  const greeter = yield* Greeter
  return greeter.greet("Ada")
}).pipe(Effect.provide(Greeter.layer))
```

## Pitfalls

- Running effects in constructors instead of layers.
- Creating fresh layers for each provide when you want shared memoization.
- If you need isolation, use `Layer.fresh` or `Effect.provide(layer, { local: true })`.
