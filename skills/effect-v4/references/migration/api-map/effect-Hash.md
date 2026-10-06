# v3 → v4 API map: effect/Hash

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Hash`

- `Hash.cached` -> `none`: Delete Hash.cached wrappers and return the computed value from Hash.symbol; Hash.hash now caches objects automatically in a private WeakMap without mutating them.
