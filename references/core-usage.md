# Core Usage (Data Types and Combinators)

Use this guide for everyday Effect composition and common data types.

## Data types
- `Option` represents optional values with `Some` or `None`. Use when a value may be absent (replace null/undefined).
- `Either` represents a value that is `Right` (success) or `Left` (failure). Use for expected errors with typed failure cases.
- `Chunk` is an immutable, indexed collection for efficient sequences. Use for building collections without mutation.
- `Duration` is a typed time value for delays and schedules. Use with `Effect.sleep` and `Schedule` combinators.
- `Equal` defines structural equality for domain types. Use to compare values by content, not reference.
- For richer domain primitives (`DateTime`, `BigDecimal`, `HashSet`, `Redacted`), see `data-types-advanced.md`.

## Common combinators
- Use `Effect.map` to transform success values, `Effect.flatMap` to chain effects, and `Effect.tap` for side effects.
- Use `Effect.gen` for imperative-style composition with `yield*` syntax.
- Use `Effect.catchAll` or `Effect.match` to handle failures and branch on success vs error.
- Use `Effect.all` to gather multiple effects; specify `concurrency` options for parallel execution.
- Use `Effect.filterOrFail` and `Effect.filterOrElse` to refine values with failure handling.

## Guidance
- Keep effects lazy; build values first and run them at the edge with `Effect.runPromise` or `Effect.runFork`.
- Prefer small, composable effects over large monoliths.
- Use `Effect.gen` for complex workflows; use direct combinators for simple transformations.
- Handle expected errors with `Either` or `Effect.catchAll`; let defects propagate for unexpected failures.
- When comparing or sorting domain values, centralize semantics via `behavior-traits.md`.

## Example

```ts
import { Effect, Either, Option, Ref } from "effect"

type AppConfig = { readonly theme: "light" | "dark"; readonly timeout: number }
type FetchError = { readonly _tag: "FetchError"; readonly message: string }

const fetchRemoteConfig = (userId: string) =>
  userId === "missing"
    ? Effect.fail<FetchError>({ _tag: "FetchError", message: "not found" })
    : Effect.succeed<AppConfig>({ theme: "dark", timeout: 45 })

const program = Effect.gen(function*() {
  const cache = yield* Ref.make(new Map<string, AppConfig>())

  const fetchConfig = (userId: string) =>
    Effect.gen(function*() {
      const cached = Option.fromNullable((yield* Ref.get(cache)).get(userId))
      if (Option.isSome(cached)) {
        return cached.value
      }

      const config = yield* fetchRemoteConfig(userId)
      yield* Ref.update(cache, (current) => {
        const next = new Map(current)
        next.set(userId, config)
        return next
      })
      return config
    })

  const result = yield* Effect.either(fetchConfig("user-1"))

  return Either.match(result, {
    onLeft: () => ({ theme: "light" as const, timeout: 30 }),
    onRight: (config) => config
  })
})
```

## Docs

- `https://effect.website/docs/getting-started/the-effect-type/`
- `https://effect.website/docs/getting-started/building-pipelines/`
- `https://effect.website/docs/getting-started/control-flow/`
- `https://effect.website/docs/data-types/option/`
- `https://effect.website/docs/data-types/either/`
