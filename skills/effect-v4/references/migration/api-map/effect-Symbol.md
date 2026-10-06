# v3 → v4 API map: effect/Symbol

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Symbol`

- `Symbol.Equivalence` -> `Equivalence.strictEqual<symbol>()`: The dedicated symbol instance was removed; it used strict equality.
