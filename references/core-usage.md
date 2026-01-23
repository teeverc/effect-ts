# Core Usage (Data Types and Combinators)

Use this guide for everyday Effect composition and common data types.

## Data types
- `Option` represents optional values with `Some` or `None`.
- `Either` represents a value that is `Right` (success) or `Left` (failure).
- `Chunk` is an immutable, indexed collection for efficient sequences.
- `Duration` is a typed time value for delays and schedules.
- `Equal` defines structural equality for domain types.

## Common combinators
- Use `Effect.map`, `Effect.flatMap`, and `Effect.tap` to transform and sequence effects.
- Use `Effect.catchAll` (or `Effect.match`) to branch on success vs failure.
- Use `Effect.all` to gather multiple effects; choose `concurrency` options for parallelism.

## Guidance
- Keep effects lazy; build values first and run them at the edge.
- Prefer small, composable effects over large monoliths.

## Example

```ts
import * as Effect from "effect/Effect"
import * as Option from "effect/Option"

const program = Effect.gen(function*() {
  const some = yield* Effect.succeedSome(42)
  const none = yield* Effect.succeedNone
  return Option.isSome(some) && Option.isNone(none)
})
```
