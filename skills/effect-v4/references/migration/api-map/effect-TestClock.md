# v3 → v4 API map: effect/TestClock

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TestClock`

- `TestClock.Data` -> `TestClock.TestClock.State`: The nearest state model is State, with timestamp and a private latch-based sleep queue. V4 exposes no full state getter or setter.

- `TestClock.adjustWith` -> `Effect.zipWith(effect, TestClock.adjust(duration), (result) => result, { concurrent: true })`: V4 removed adjustWith. Run the tested effect and clock adjustment concurrently and retain the tested effect's result.

- `TestClock.currentTimeMillis` -> `Clock.currentTimeMillis`: Read time from the active Clock reference; under it.effect or TestClock.layer() this is virtual time.

- `TestClock.defaultTestClock` -> `TestClock.layer()`: The v4 layer creates an epoch-based test clock and captures the surrounding live Clock automatically; it no longer needs TestAnnotations or TestLive.

- `TestClock.makeData` -> `TestClock.layer() + TestClock.setTime(instant)`: State injection was removed. Build the layer, then set initial time; seeded pending sleeps cannot migrate because the queue is private.

- `TestClock.save` -> `none`: Full clock snapshots including pending sleeps are no longer public. For timestamp-only restoration, read Clock.currentTimeMillis and later call TestClock.setTime(savedMillis).

- `TestClock.sleeps` -> `none`: The pending-sleep queue is private. Test observable behavior by forking sleepers, adjusting time, and joining or asserting the fibers.

- `TestClock.testClock` -> `TestClock.testClockWith(Effect.succeed)`: V4 exposes callback-based access to the active test clock; use testClockWith directly when possible.
