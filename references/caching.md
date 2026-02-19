# Caching (Cache, cached)

Use this guide when memoizing effects or sharing computed values.

## Mental model

- `Effect.cached` memoizes a single effect result.
- `Cache` offers key-based storage with TTL and capacity policies.

## Patterns

- Use `Effect.cached` for a single expensive effect.
- Use `Cache.make` for memoizing lookups by key.
- Use `Cache.makeWith` when TTL depends on the result or key.

## Walkthrough: memoize by key with Cache

```ts
import * as Cache from "effect/Cache"
import * as Effect from "effect/Effect"
import * as Random from "effect/Random"

const program = Effect.gen(function*() {
  const cache = yield* Cache.make<string, { id: string; n: number }>({
    capacity: 100,
    lookup: (id) =>
      Random.nextIntBetween(1, 100).pipe(Effect.map((n) => ({ id, n })))
  })

  const first = yield* Cache.get(cache, "user-1")
  const second = yield* Cache.get(cache, "user-1")

  return [first, second]
})
```

## Wiring guide

- Use TTL caches for volatile data; use size caps for unbounded key spaces.
- Expose caches as services via layers if many modules need them.
- Invalidate or refresh caches at boundaries (config change, deploy, etc.).

## Pitfalls

- Caching non-deterministic effects without an explicit strategy.
- Unbounded cache growth with high-cardinality keys.
- Forgetting to handle cache invalidation or TTLs.
