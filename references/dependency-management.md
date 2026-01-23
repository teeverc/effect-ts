# Dependency Management (Services, Context, Layers)

Use this guide when modeling dependencies and wiring services.

- **Service**: a dependency represented by a TypeScript interface.
- **Tag**: a typed identifier that points to a service instance.
- **Context**: a map of tags to concrete service implementations.
- **Layer**: the abstraction for constructing services and managing their dependencies during construction.

## Patterns
- Define services via `Context.Tag` when you want a custom shape.
- Use `Effect.Service` for the common pattern where you want a service class plus an auto-generated tag, accessors, and a default layer.
- Keep construction concerns (resource acquisition, config, wiring) inside layers so service interfaces stay clean.
- Compose layers to build dependency graphs and provide the environment at program startup.

## Example
```ts
import * as Context from "effect/Context"
import * as Effect from "effect/Effect"
import * as Layer from "effect/Layer"

class Clock extends Context.Tag("Clock")<Clock, { readonly now: Effect.Effect<number> }>() {
  static Live = Layer.succeed(Clock, { now: Effect.sync(() => Date.now()) })
}

const program = Effect.gen(function*() {
  const clock = yield* Clock
  const now = yield* clock.now
  return now
})

Effect.runPromise(program.pipe(Effect.provide(Clock.Live)))
```
