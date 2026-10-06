# Caching (Effect.cached*, Cache, ScopedCache, PersistedCache)

Use this guide when memoizing effects or sharing keyed lookup results. Request-level batching + caching: `references/request-resolver.md`. Ref-counted shared resources: `RcRef` / `RcMap` (`references/resource-management.md`).

## Choose a tool

| Need | API |
| --- | --- |
| Memoize one effect for the program's lifetime | `Effect.cached(effect)` |
| Memoize one effect with expiry | `Effect.cachedWithTTL(effect, ttl \| (exit) => ttl)` |
| ...and invalidate manually | `Effect.cachedInvalidateWithTTL(effect, ttl)` → `[cached, invalidate]` |
| Keyed lookups with capacity/TTL | `Cache.make({ lookup, capacity, timeToLive? })` |
| Keyed lookups whose values own resources | `ScopedCache.make(...)` (needs `Scope`) |
| Durable cache across restarts/workers | `PersistedCache.make` from `effect/persistence` (`@stability unstable`) |
| Cache results of a batched resolver | `RequestResolver.withCache` / `RequestResolver.asCache` |

`Effect.cachedFunction` and `Effect.once` were removed: use `Cache` for keyed memoization and `Effect.cached` for once-only (`references/migration/api-map/effect-Effect.md`).

## Effect.cached*

```ts
import { Effect, Exit } from "effect"

const program = Effect.gen(function*() {
  const loadConfig = yield* Effect.cached(fetchRemoteConfig)      // Effect<Effect<A, E, R>>
  yield* loadConfig                                               // runs fetchRemoteConfig
  yield* loadConfig                                               // reuses the result

  const token = yield* Effect.cachedWithTTL(fetchToken, "5 minutes")
  // TTL chosen per result: cache failures briefly
  const flaky = yield* Effect.cachedWithTTL(fetchFlags, (exit) => Exit.isSuccess(exit) ? "1 minute" : "5 seconds")

  const [rates, invalidateRates] = yield* Effect.cachedInvalidateWithTTL(fetchRates, "1 hour")
  yield* invalidateRates
})
```

## Cache

```ts
import { Cache, Data, Duration, Effect, Exit } from "effect"

class UserId extends Data.Class<{ id: number }> {}   // keys use Equal/Hash; Data classes compare by value

const program = Effect.gen(function*() {
  const users = yield* Cache.make({
    capacity: 1000,
    lookup: (key: UserId) => fetchUser(key.id),
    timeToLive: Duration.minutes(5)                    // default: infinite
  })

  const user = yield* Cache.get(users, new UserId({ id: 1 }))  // runs lookup on miss; concurrent callers share it
  yield* Cache.set(users, new UserId({ id: 2 }), someUser)
  yield* Cache.invalidate(users, new UserId({ id: 1 }))
  yield* Cache.refresh(users, new UserId({ id: 3 }))           // recompute, replace on completion
  const hit = yield* Cache.getOption(users, new UserId({ id: 2 }))   // Option, no lookup; fails if cached failure
  const ok = yield* Cache.getSuccess(users, new UserId({ id: 2 }))   // Option of successful value only
  yield* Cache.invalidateWhen(users, new UserId({ id: 2 }), (u) => u.disabled)
  yield* Cache.invalidateAll(users)
  // also: Cache.has, Cache.size, Cache.keys, Cache.values, Cache.entries
})

// TTL from the result and key
const byExit = Cache.makeWith((key: string) => lookup(key), {
  capacity: 100,
  timeToLive: (exit, _key) => Exit.isFailure(exit) ? "10 seconds" : "10 minutes"
})
```

- Failures are cached too (for their TTL). Use `makeWith` with an `Exit`-based TTL (or `Duration.zero` for failures) to avoid caching errors.
- `requireServicesAt`: by default the lookup's services are captured when the cache is constructed (`R` on `Cache.make`). Pass `requireServicesAt: "lookup"` to require them at each `Cache.get` instead.
- Capacity eviction is LRU-style; a zero TTL means "do not retain".

## ScopedCache

```ts
import { Effect, ScopedCache } from "effect"

const program = Effect.scoped(Effect.gen(function*() {
  const conns = yield* ScopedCache.make({
    capacity: 16,
    timeToLive: "10 minutes",
    lookup: (host: string) => Effect.acquireRelease(openConnection(host), (c) => c.close)
  })
  const conn = yield* ScopedCache.get(conns, "db-1")
}))
```

Each entry owns a `Scope`, released when the entry is evicted, invalidated, expires, or the outer scope closes. Same API surface as `Cache` (`get`, `getOption`, `getSuccess`, `set`, `has`, `invalidate`, `invalidateWhen`, `refresh`, `invalidateAll`, `size`, `keys`, `values`, `entries`).

## Interruption semantics (changed in 4.0.1)

`Effect.cached`, `cachedWithTTL`, `cachedInvalidateWithTTL`, `Cache`, `ScopedCache`, `RcRef`, `RcMap` treat interruption as **abandonment**:

- Concurrent callers share one in-flight computation; it is interrupted only after **every** waiting caller has been interrupted.
- Interrupted results are never cached.
- A call made while an abandoned computation is still finalizing starts a fresh computation.
- Missing-key lookups run in daemon fibers: children forked by a lookup live as long as the lookup, not the caller.

So a single caller timing out (`Effect.timeout`) does not poison or cancel the lookup for others.

## PersistedCache (`effect/persistence`, unstable)

```ts
import { Effect, Schema } from "effect"
import { Persistable, PersistedCache, Persistence } from "effect/persistence"

class GetUser extends Persistable.Class<{ payload: { id: number } }>()("GetUser", {
  primaryKey: (p) => `GetUser:${p.id}`,
  success: User,
  error: UserNotFound
}) {}

const program = Effect.gen(function*() {
  const cache = yield* PersistedCache.make((req: GetUser) => fetchUser(req.id), {
    storeId: "users",
    timeToLive: (exit, req) => "1 hour",       // persisted TTL
    inMemoryCapacity: 1024,                     // default 1024
    inMemoryTTL: () => "10 seconds"             // default 10s
  })
  const user = yield* cache.get(new GetUser({ id: 1 }))
  yield* cache.invalidate(new GetUser({ id: 1 }))
}).pipe(
  Effect.scoped,
  Effect.provide(Persistence.layerMemory)       // or layerKvs, layerRedis, layerSql, layerSqlMultiTable
)
```

Results are stored as encoded `Exit`s using the request's `success`/`error` schemas; lookups fail with `PersistenceError | SchemaError` in addition to the request error.

## Pitfalls

- Recreating the cache per call (`Effect.cached` inside a function body): build it once in a Layer and share it.
- Caching non-deterministic or user-specific results under an unscoped key.
- High-cardinality keys without `capacity`/`timeToLive`.
- Accidentally caching failures for the full TTL; use `makeWith` / `cachedWithTTL` with an `Exit`-based TTL.
- Plain object keys compare by reference; use `Data.Class`, `Schema.Class` instances, primitives, or strings.
