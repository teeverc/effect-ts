# v3 → v4 API map: effect/MutableHashSet

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MutableHashSet`

- `MutableHashSet.MutableHashSet` -> `MutableHashSet.MutableHashSet`: The MutableHashSet model remains; use its public operations rather than depending on internal representation fields.

- `MutableHashSet.TypeId` -> `none`: The public MutableHashSet.TypeId was removed; the v4 marker is private.
