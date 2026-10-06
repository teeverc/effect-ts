# v3 → v4 API map: effect/Pool

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Pool`

- `Pool.Pool` -> `Pool.Pool`: The model remains but is now Pipeable rather than an Effect subtype; borrow resources explicitly with Pool.get.

- `Pool.Pool.Variance` -> `none`: The public Pool variance marker was removed; use Pool.Pool directly.

- `Pool.PoolTypeId` -> `none`: The Pool type id is internal in v4; use Pool.isPool for runtime refinement.

- `Pool.PoolUnify` -> `none`: Pool is no longer an Effect subtype, so its Effect unification helper was removed; call Pool.get explicitly.

- `Pool.PoolUnifyIgnore` -> `none`: Pool is no longer an Effect subtype, so its Effect unification ignore marker was removed.
