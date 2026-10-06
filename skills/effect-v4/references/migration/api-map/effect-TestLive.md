# v3 → v4 API map: effect/TestLive

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TestLive`

- `TestLive.TestLive` -> `none`: There is no grouped live-default-services object. Use Context.Context plus Effect.provideContext, TestClock.withLive for live time, or it.live for the whole test.

- `TestLive.TestLiveTypeId` -> `none`: The TestLive nominal wrapper was removed, so its type id has no replacement.

- `TestLive.make` -> `Effect.provideContext`: The wrapper was removed. Apply a captured Context directly with Effect.provideContext; for live time inside it.effect, prefer TestClock.withLive.
