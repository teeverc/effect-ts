# v3 → v4 API map: effect/Random

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Random`

- `Random.Random` -> `Random.Random`: The context key is now a Context.Reference whose low-level service only has nextIntUnsafe and nextDoubleUnsafe. Prefer module operations; custom providers implement those two primitives.

- `Random.RandomTypeId` -> `none`: The service is structural and no longer carries a public RandomTypeId brand.

- `Random.fixed` -> `Effect.provideService(Random.Random, customRandom)`: No exact built-in equivalent remains. For deterministic tests, provide a cycling service implementing nextIntUnsafe and nextDoubleUnsafe; map non-number sequences explicitly.

- `Random.make` -> `Random.withSeed`: Replace service construction and withRandom with Random.withSeed(seed)(program). V4 accepts string or number, returns an Effect transformation, and uses a different PRNG, so sequences are not v3-compatible.

- `Random.nextRange` -> `Random.nextBetween`: Direct rename; both produce a floating-point value in the half-open range [min, max).

- `Random.randomWith` -> `Random.Random.use`: Use Random.Random.use for raw service access. Prefer replacing callbacks that selected an old method with the corresponding module-level Random operation.
