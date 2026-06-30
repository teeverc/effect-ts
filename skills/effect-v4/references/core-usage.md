# Core Usage (Data Types and Combinators) - v4

Use this guide for everyday Effect composition and common data types in Effect v4.

## Data types
- `Option` represents optional values with `Some` or `None`. Use when a value may be absent (replace null/undefined).
- `Result` represents a value that is `Success` (success) or `Failure` (failure). Use for expected errors with typed failure cases; replaces `Either` from v3.
- `Chunk` is an immutable, indexed collection for efficient sequences. Use for building collections without mutation.
- `Duration` is a typed time value for delays and schedules. Use with `Effect.sleep` and `Schedule` combinators.
- `Equal` defines structural equality for domain types. Use to compare values by content, not reference.

## Common combinators
- Use `Effect.map` to transform success values, `Effect.flatMap` to chain effects, and `Effect.tap` for side effects.
- Use `Effect.gen` for imperative-style composition with `yield*` syntax.
- Use `Effect.catch` (v3: `Effect.catchAll`) or `Effect.match` to handle failures and branch on success vs error.
- Use `Effect.all` to gather multiple effects; specify `concurrency` options for parallel execution.
- Use `Effect.filterOrFail` and `Effect.filterOrElse` to refine values with failure handling.
- Use `Effect.firstSuccessOf` when trying alternatives until one succeeds.
- Use `Effect.transposeOption` when turning `Option<Effect<A, E, R>>` into `Effect<Option<A>, E, R>`.
- Use `Effect.fromOption` with custom error callbacks when `None` should map to domain-specific failures.
- Use `Effect.abortSignal` at Promise / fetch boundaries that should be interrupted by the current fiber.
- Use `Effect.acquireDisposable` for resources that expose disposal semantics.

## Guidance
- Keep effects lazy; build values first and run them at the edge with `Effect.runPromise` or `Effect.runFork`.
- Prefer small, composable effects over large monoliths.
- Use `Effect.gen` for complex workflows; use direct combinators for simple transformations.
- Handle expected errors with `Result` or `Effect.catch`; let defects propagate for unexpected failures.

## Example (v4)

```ts
import * as Effect from "effect/Effect"
import * as Result from "effect/Result"
import * as Option from "effect/Option"
import * as Duration from "effect/Duration"

// Fetch user config with optional cache timeout
const fetchConfig = (userId: string) =>
  Effect.gen(function*() {
    const cached = yield* getCachedConfig(userId)

    if (Option.isSome(cached)) {
      return cached.value
    }

    const result = yield* Effect.tryPromise({
      try: () => fetch(`/api/config/${userId}`).then(r => r.json()),
      catch: (error) => ({ _tag: "FetchError" as const, error })
    })

    yield* cacheConfig(userId, result).pipe(
      Effect.tap(() => Effect.sleep(Duration.seconds(60)))
    )

    return result
  })

// Handle errors with Result branching (v3 used Either)
const getConfigOrDefault = (userId: string) =>
  Effect.gen(function*() {
    const result = yield* Effect.result(fetchConfig(userId))

    return Result.match(result, {
      onFailure: () => ({ theme: "light", timeout: 30 }),
      onSuccess: (config) => config
    })
  })
```

## v3 → v4 API Changes (Quick Reference)
| v3 | v4 |
|----|-----|
| `Either` | `Result` |
| `Effect.catchAll` | `Effect.catch` |
| `Effect.catchAllCause` | `Effect.catchCause` |
| `Effect.catchSome` | `Effect.catchFilter` |

## Recent Beta Notes

- `Effect.try` and `Effect.tryPromise` received fixes around thunk usage, error mapping, and abort signal handling. Prefer the object form when you need a typed `catch` mapper or access to a signal.
- Module-level side effects were made more tree-shakable; avoid patterns that rely on import-time work.
- `Random.choice` was ported from v3 for selecting a random element from an iterable.

See `references/migration/error-handling.md` for detailed error handling changes.
