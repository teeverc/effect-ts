# v3 → v4 API map: effect/KeyedPool

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/KeyedPool`

- `KeyedPool.KeyedPool` -> `RcMap.RcMap<K, Pool.Pool<A, E>>`: Model keyed pools as an RcMap whose scoped lookup creates one Pool per key.

- `KeyedPool.KeyedPool.Variance` -> `none`: KeyedPool and its variance marker were removed; use the RcMap and Pool public models without depending on branding internals.

- `KeyedPool.KeyedPoolTypeId` -> `none`: KeyedPool was removed, so its runtime type id has no v4 equivalent.

- `KeyedPool.get` -> `RcMap.get + Pool.get`: KeyedPool was removed; acquire the per-key Pool from an RcMap, then borrow an item with Pool.get in the current Scope.

- `KeyedPool.invalidate` -> `RcMap.get + Pool.invalidate`: KeyedPool was removed; retain the key, get its Pool from RcMap, and call Pool.invalidate for the item.

- `KeyedPool.make` -> `RcMap.make + Pool.make`: Create an RcMap with lookup key =\> Pool.make({ acquire: acquire(key), size }); RcMap.get followed by Pool.get replaces keyed borrowing.

- `KeyedPool.makeWith` -> `RcMap.make + Pool.make`: Create an RcMap whose lookup uses Pool.make with size: size(key).

- `KeyedPool.makeWithTTL` -> `RcMap.make + Pool.makeWithTTL`: Create an RcMap whose lookup uses Pool.makeWithTTL with min(key), max(key), and the shared timeToLive.

- `KeyedPool.makeWithTTLBy` -> `RcMap.make + Pool.makeWithTTL`: Create an RcMap whose lookup uses Pool.makeWithTTL with min(key), max(key), and timeToLive(key).
