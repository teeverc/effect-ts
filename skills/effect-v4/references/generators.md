# Generators (Effect.gen) - v4

Use this guide when sequential logic would be clearer than pipelines.

## Mental model

- `Effect.gen` is async/await-style control flow for Effects.
- `yield*` extracts values from effects and Yieldables in order.
- The error channel short-circuits just like thrown errors in async/await.
- **v4 change:** Many values are Yieldable but not Effects. Use module functions like `Ref.get`, `Deferred.await`, `Fiber.join` instead of yielding the raw values.

## Patterns

- Prefer generators for multi-step workflows and branching.
- Keep small effects for each step and compose with `yield*`.
- Use `Effect.catch` (v3: `Effect.catchAll`) or `Effect.catchTag` at the boundary for recovery.
- If a value is Yieldable but not an Effect, call `.asEffect()` before using Effect combinators.

## Walkthrough: sequential flow with branching

```ts
import * as Effect from "effect/Effect"

const lookup = (id: string) =>
  id === "guest" ? Effect.succeed({ id }) : Effect.fail("not found")

const program = Effect.gen(function*() {
  const user = yield* lookup("guest")

  if (user.id === "guest") {
    return "welcome"
  }

  return "hello"
}).pipe(Effect.catch(() => Effect.succeed("fallback")))
```

## Practical Example: Multi-step orchestration with state

```ts
import * as Effect from "effect/Effect"
import * as Ref from "effect/Ref"
import * as ServiceMap from "effect/ServiceMap"

interface Logger {
  log: (msg: string) => Effect.Effect<void>
}

const Logger = ServiceMap.Service<Logger>("Logger")

const orchestrate = Effect.gen(function*() {
  const logger = yield* Logger
  const counter = yield* Ref.make(0)

  // Step 1: fetch data
  yield* logger.log("Starting fetch...")
  const data = yield* Effect.tryPromise(() =>
    fetch("/api/data").then(r => r.json())
  )

  // Step 2: update state and log
  yield* Ref.update(counter, n => n + 1)
  const count = yield* Ref.get(counter)
  yield* logger.log(`Processed ${count} item(s)`)

  return data
})
```

## Practical Example: Working with Yieldable (v4 change)

In v4, many values are Yieldable but not Effects. Use module functions to access them:

```ts
import * as Effect from "effect/Effect"
import * as Ref from "effect/Ref"
import * as Deferred from "effect/Deferred"
import * as Fiber from "effect/Fiber"

const workflow = Effect.gen(function*() {
  // ✓ Ref.get returns a Yieldable
  const counter = yield* Ref.make(0)
  const current = yield* Ref.get(counter)

  // ✓ Deferred.await returns a Yieldable
  const deferred = yield* Deferred.make<string, Error>()
  const value = yield* Deferred.await(deferred)

  // ✓ Fiber.join returns a Yieldable
  const fiber = yield* Effect.forkChild(Effect.sleep(1000))
  yield* Fiber.join(fiber)

  return current
})

// If you need to feed a Yieldable into Effect combinators, use .asEffect()
const refAsEffect = Ref.get(yield* Ref.make(0)).asEffect()
```

## Pitfalls

- Nesting generators unnecessarily instead of extracting helpers.
- Throwing exceptions in generators instead of failing effects.
- Using `Effect.gen` when a simple pipeline is clearer.
- Yielding non-yieldable values such as `Ref` or `Fiber` directly (use `Ref.get` / `Fiber.join`).
- Forgetting to call `.asEffect()` when passing Yieldable values to Effect combinators like `Effect.all` or `Effect.flatMap`.

See `references/migration/generators.md` and `references/migration/yieldable.md` for detailed v3 → v4 changes.
