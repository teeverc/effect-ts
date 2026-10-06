# v3 → v4 API map: effect/ScopedRef

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/ScopedRef`

- `ScopedRef.ScopedRef` -> `ScopedRef.ScopedRef`: The type remains but no longer extends Effect; use ScopedRef.get or ScopedRef.getUnsafe.

- `ScopedRef.ScopedRef.Variance` -> `none`: The exported variance artifact was removed.

- `ScopedRef.ScopedRefTypeId` -> `none`: The marker is private in v4 and no public guard exists.

- `ScopedRef.ScopedRefUnify` -> `none`: ScopedRef no longer extends Effect; use ScopedRef.get explicitly.

- `ScopedRef.ScopedRefUnifyIgnore` -> `none`: The Effect-unification implementation detail was removed.
