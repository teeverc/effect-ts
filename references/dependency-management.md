# Dependency Management (Services, ServiceMap, Layers)

Use this guide when modeling dependencies and wiring services in Effect v4.

- **Service**: a dependency represented by a TypeScript interface.
- **ServiceMap**: a map of service identifiers to concrete implementations.
- **ServiceMap.Service**: a typed identifier (and optional constructor) for a service.
- **Layer**: the abstraction for constructing services and managing their dependencies during construction.

## Patterns
- Define services via `ServiceMap.Service` (function or class syntax).
- Prefer `yield*` to access services inside `Effect.gen`; `ServiceMap.Service.use` is fine for small inline access.
- Use `Layer.succeed` for pure values and `Layer.effect`/`Layer.scoped` when construction is effectful.
- Keep construction concerns (resource acquisition, config, wiring) inside layers so service interfaces stay clean.
- Compose layers with `Layer.merge`/`Layer.provide` to build dependency graphs and provide the environment at program startup.

## Example

```ts
import { Effect, Layer, ServiceMap } from "effect"

interface Config {
  readonly prefix: string
}

const Config = ServiceMap.Service<Config>("Config")
const ConfigLayer = Layer.succeed(Config, { prefix: "PRE" })

class Greeter extends ServiceMap.Service<Greeter>()("Greeter", {
  make: Effect.gen(function* () {
    const config = yield* Config
    return {
      greet: (name: string) => `${config.prefix} ${name}`
    }
  })
}) {
  static layer = Layer.effect(this, this.make).pipe(
    Layer.provide(ConfigLayer)
  )
}

const program = Effect.gen(function* () {
  const greeter = yield* Greeter
  return greeter.greet("Ada")
}).pipe(Effect.provide(Greeter.layer))
```
