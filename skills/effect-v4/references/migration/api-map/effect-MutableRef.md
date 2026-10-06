# v3 → v4 API map: effect/MutableRef

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MutableRef`

- `MutableRef.MutableRef` -> `MutableRef.MutableRef`: The MutableRef model remains; use its public operations rather than depending on internal representation fields.

- `MutableRef.TypeId` -> `none`: The public MutableRef.TypeId was removed; the v4 marker is private.
