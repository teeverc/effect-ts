# v3 → v4 API map: effect/RcMap

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/RcMap`

- `RcMap.RcMap` -> `RcMap.RcMap`: The model remains as a Pipeable reference-counted resource map; use RcMap.get explicitly inside a Scope.

- `RcMap.RcMap.Variance` -> `none`: The public variance marker was removed; use RcMap.RcMap directly.

- `RcMap.TypeId` -> `none`: The RcMap type id is internal in v4; do not inspect or construct the brand directly.
