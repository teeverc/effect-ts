# v3 → v4 API map: effect/Utils

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Utils`

- `Utils.Adapter` -> `none`: The generator-adapter type was removed; type generator bodies to yield v4 yieldable values directly.

- `Utils.Gen` -> `Utils.Gen`: Still exported; drop the adapter type and resume parameter, then yield yieldable Kind values directly.

- `Utils.GenKind` -> `none`: The adapter wrapper was removed; custom yieldable Kinds should implement Symbol.iterator and return Utils.SingleShotGen.

- `Utils.GenKindImpl` -> `none`: The wrapper implementation was removed; implement direct yieldability with Symbol.iterator and Utils.SingleShotGen.

- `Utils.GenKindTypeId` -> `none`: The GenKind runtime marker was removed with the wrapper infrastructure.

- `Utils.OptionalNumber` -> `number | null | undefined`: The unused named alias was removed; inline its union.

- `Utils.PCGRandom` -> `Random.withSeed + Random.next / Random.nextIntBetween`: Use the effectful Random service for seeded generation; v4 is not PCG-compatible.

- `Utils.PCGRandomState` -> `none`: No public PCG state snapshot or restore API remains; Random.withSeed is reproducible but not state-compatible.

- `Utils.SingleShotGen` -> `Utils.SingleShotGen`: Still exported; v4 removes its concrete return and throw methods, so do not call those optional iterator hooks.

- `Utils.Variance` -> `Utils.Variance`: Still exported; remove the v3 GenKindTypeId marker from implementations.

- `Utils.YieldWrap` -> `none`: The internal generator transport wrapper was removed; yieldable values are yielded directly.

- `Utils.YieldWrapTypeId` -> `none`: The internal wrapper marker was removed with YieldWrap.

- `Utils.adapter` -> `none`: Remove the adapter and resume parameter; v4 generators yield yieldable values directly.

- `Utils.internalCall` -> `none`: This was internal and has no public replacement; application code should invoke its thunk directly.

- `Utils.isGenKind` -> `none`: Removed with GenKind; v4 generator drivers consume directly yielded values.

- `Utils.isGeneratorFunction` -> `none`: The unused constructor-identity predicate was removed; accept an explicit generator contract instead.

- `Utils.makeGenKind` -> `none`: The wrapper constructor was removed; make custom Kinds yieldable with Symbol.iterator and Utils.SingleShotGen.

- `Utils.structuralRegion` -> `none`: Remove the wrapper because v4 Equal.equals is structural by default; use a custom Equivalence for custom comparison.

- `Utils.structuralRegionState` -> `none`: The mutable test hook was removed; v4 equality is structural by default.

- `Utils.yieldWrapGet` -> `none`: The internal unwrapper was removed; generator drivers read the directly yielded value.
