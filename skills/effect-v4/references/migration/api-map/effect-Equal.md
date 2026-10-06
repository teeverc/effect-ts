# v3 → v4 API map: effect/Equal

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Equal`

- `Equal.equivalence` -> `Equal.asEquivalence`: Direct rename. The returned equivalence now follows v4 structural equality, including NaN equality and cached comparisons for immutable objects.
