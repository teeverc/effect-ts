# Layer Patterns - v4

Use this guide when wiring services and environments.

## Mental model

- Layers build dependency graphs and manage construction.
- Use `Layer.scoped` for resources with lifetimes.
- Provide layers at app boundaries and tests.
- In v4, layers are memoized across `Effect.provide` calls by default (global shared `MemoMap`).

## v4 Change: Automatic Cross-Provide Memoization

In v3, layers were only memoized within a single `Effect.provide` call. Two separate `provide` calls with the same layer would build it twice. In v4, layer memoization is shared globally across all `provide` calls:

```ts
// v4: "Building MyService" is logged ONCE even with two provide calls
const main = program.pipe(
  Effect.provide(MyServiceLayer),
  Effect.provide(MyServiceLayer)
)
```

This is a safety net — **still prefer composing layers before providing:**

```ts
// Preferred — provide once
const main = program.pipe(Effect.provide(MyServiceLayer))
```

## Opting Out of Shared Memoization

### `Layer.fresh`

Forces the layer to always build with a fresh memo map, bypassing the shared cache:

```ts
import { Effect, Layer } from "effect"

const main = program.pipe(
  Effect.provide(MyServiceLayer),
  Effect.provide(Layer.fresh(MyServiceLayer))
)
// Builds twice — fresh bypasses the shared cache
```

### `Effect.provide` with `{ local: true }` (New in v4)

Builds the layer with a local memo map instead of the shared one. The layer and all its sublayers are built from scratch:

```ts
import { Effect } from "effect"

const main = program.pipe(
  Effect.provide(MyServiceLayer),
  Effect.provide(MyServiceLayer, { local: true })
)
// Builds twice — local creates its own memo map
```

Use `local: true` for test isolation where each test should get independent resources.

## Patterns

- Use `Layer.succeed` for pure values.
- Use `Layer.effect` or `Layer.scoped` for effectful acquisition.
- Combine with `Layer.merge` and provide with `Effect.provide`.
- Use `Layer.fresh` or `{ local: true }` when isolation is required (tests, independent pools).

## Walkthrough: service + layer (v4)

```ts
import * as Effect from "effect/Effect"
import * as Layer from "effect/Layer"
import * as ServiceMap from "effect/ServiceMap"

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
- Creating a fresh layer instance per use when shared memoization is desired.
- Assuming v3 behavior where the same layer in two `provide` calls is built twice.

## Docs

- `https://effect.website/docs/requirements-management/layers/`
- `https://effect.website/docs/requirements-management/layer-memoization/`
