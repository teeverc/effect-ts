# Generators (Effect.gen) - v4

Use this guide when sequential logic would be clearer than pipelines.

## Mental model

- `Effect.gen` is async/await-style control flow for Effects.
- `yield*` extracts values from `Effect` values in order.
- The error channel short-circuits just like thrown errors in async/await.
- **Current beta:** `Effect.Yieldable` was removed. Use module functions like `Ref.get`, `Deferred.await`, `Fiber.join`, or `Effect.service` instead of yielding raw data handles.
- Service keys created with `Context.Service` / `Context.Reference` can still be yielded because their implementation is Effect-like.
- `Config` values are also Effect-like and can be yielded to load from the current `ConfigProvider`.

## Patterns

- Prefer generators for multi-step workflows and branching.
- Keep small effects for each step and compose with `yield*`.
- Use `Effect.catch` (v3: `Effect.catchAll`) or `Effect.catchTag` at the boundary for recovery.
- If you need custom Effect-like values, use `Effectable.Class` or `Effectable.Prototype`; ordinary values should expose explicit functions that return `Effect`.

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
import * as Context from "effect/Context"

interface Logger {
  log: (msg: string) => Effect.Effect<void>
}

const Logger = Context.Service<Logger>("Logger")

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

## Practical Example: Reading Handles Explicitly

In current v4 betas, data handles are plain values. Use module functions to access or await them:

```ts
import * as Effect from "effect/Effect"
import * as Ref from "effect/Ref"
import * as Deferred from "effect/Deferred"
import * as Fiber from "effect/Fiber"

const workflow = Effect.gen(function*() {
  const counter = yield* Ref.make(0)
  const current = yield* Ref.get(counter)

  const deferred = yield* Deferred.make<string, Error>()
  const value = yield* Deferred.await(deferred)

  const fiber = yield* Effect.forkChild(Effect.sleep(1000))
  yield* Fiber.join(fiber)

  return { current, value }
})
```

## Custom Effect-like Values

```ts
import * as Effect from "effect/Effect"
import * as Effectable from "effect/Effectable"

class CurrentTime extends Effectable.Class<number> {
  override = Effect.sync(() => Date.now())
}

const program = Effect.gen(function*() {
  return yield* new CurrentTime()
})
```

## Pitfalls

- Nesting generators unnecessarily instead of extracting helpers.
- Throwing exceptions in generators instead of failing effects.
- Using `Effect.gen` when a simple pipeline is clearer.
- Yielding plain values such as `Ref`, `Deferred`, or `Fiber` directly instead of `Ref.get`, `Deferred.await`, or `Fiber.join`.
- Recommending removed `Effect.Yieldable` or `.asEffect()` in current v4 guidance.

See `references/migration/generators.md` and `references/migration/yieldable.md` for detailed v3 → v4 changes.
