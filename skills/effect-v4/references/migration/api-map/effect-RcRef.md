# v3 → v4 API map: effect/RcRef

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/RcRef`

- `RcRef.RcRef` -> `RcRef.RcRef`: The model remains but is now only Pipeable; replace yielding or reading the RcRef directly with RcRef.get in a Scope.

- `RcRef.RcRefUnify` -> `none`: RcRef is no longer an Effect subtype, so its Effect unification helper was removed; call RcRef.get explicitly.

- `RcRef.RcRefUnifyIgnore` -> `none`: RcRef is no longer an Effect subtype, so its Effect unification ignore marker was removed.

- `RcRef.TypeId` -> `none`: The RcRef type id is internal in v4; do not inspect or construct the brand directly.
