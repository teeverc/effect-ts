# v3 → v4 API map: effect/MutableHashMap

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MutableHashMap`

- `MutableHashMap.MutableHashMap` -> `MutableHashMap.MutableHashMap`: The MutableHashMap model remains; use its public operations rather than depending on internal representation fields.

- `MutableHashMap.TypeId` -> `none`: The public MutableHashMap.TypeId was removed; the v4 marker is private.
