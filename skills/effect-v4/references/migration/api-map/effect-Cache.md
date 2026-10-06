# v3 → v4 API map: effect/Cache

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Cache`

- `Cache.Cache` -> `Cache.Cache`: The cache model remains, but v4 exposes a Pipeable value with explicit Cache operations and adds a lookup environment parameter.

- `Cache.Cache.ConsumerVariance` -> `none`: The ConsumerCache view and its variance marker were removed; expose a narrower application interface around Cache operations when write access must be hidden.

- `Cache.Cache.Variance` -> `none`: The public variance marker was removed; use Cache.Cache directly and do not depend on its branding internals.

- `Cache.CacheStats` -> `none`: Built-in hit and miss statistics were removed; instrument the lookup and Cache.get calls explicitly, and use Cache.size for the current entry count.

- `Cache.CacheTypeId` -> `none`: The cache type id is internal in v4; do not inspect or construct the cache brand directly.

- `Cache.ConsumerCache` -> `Cache.Cache`: ConsumerCache was removed; use Cache.Cache and expose an application-defined read-only wrapper if capability restriction is required.

- `Cache.ConsumerCacheTypeId` -> `none`: ConsumerCache and its type id were removed with the read-only cache view.

- `Cache.EntryStats` -> `none`: Per-entry loaded-time statistics were removed; record lookup timing in application instrumentation if needed.

- `Cache.Lookup` -> `(key: Key) => Effect.Effect<A, E, R>`: The named alias was removed; use an inline lookup function type or Cache.Cache\<Key, A, E, R\>["lookup"].

- `Cache.makeCacheStats` -> `none`: CacheStats and its constructor were removed; define an application metrics record if these counters are still required.

- `Cache.makeEntryStats` -> `none`: EntryStats and its constructor were removed; capture lookup timing in application instrumentation instead.
