# v3 → v4 API map: effect/Ordering

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Ordering`

- `Ordering.combineAll` -> `Ordering.Reducer.combineAll`: The combination operation moved to the exported Reducer; first-nonzero and empty-input behavior are unchanged.

- `Ordering.combineMany` -> `Ordering.Reducer.combineAll(Iterable.prepend(collection, self))`: Prepend the initial ordering before reducing to preserve v3 short-circuiting without consuming collection when self is nonzero.
