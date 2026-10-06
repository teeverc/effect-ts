# v3 → v4 API map: effect/Struct

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Struct`

- `Struct.entries` -> `Object.entries`: Use the native helper, adding a cast when the old precise key and value type is required.

- `Struct.getEquivalence` -> `Struct.makeEquivalence`: Direct rename; the fields object call shape is unchanged.

- `Struct.getOrder` -> `Struct.makeOrder`: Direct rename; the fields object call shape is unchanged.
