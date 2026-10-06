# v3 → v4 API map: effect/Resource

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Resource`

- `Resource.Resource` -> `Resource.Resource`: The type remains but no longer extends Effect; use Resource.get(resource).

- `Resource.Resource.Variance` -> `none`: The exported variance artifact was removed.

- `Resource.ResourceTypeId` -> `Resource.isResource`: The marker is private; use the public runtime guard.

- `Resource.ResourceUnify` -> `none`: Resource no longer extends Effect; use Resource.get explicitly.

- `Resource.ResourceUnifyIgnore` -> `none`: The Effect-unification implementation detail was removed.
