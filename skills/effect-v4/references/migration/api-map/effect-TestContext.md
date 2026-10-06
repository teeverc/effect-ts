# v3 → v4 API map: effect/TestContext

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TestContext`

- `TestContext.LiveContext` -> `@effect/vitest#live`: Default runtime references are live in v4. Use it.live for a whole live test; no LiveContext layer is required.

- `TestContext.TestContext` -> `Layer.mergeAll(TestConsole.layer, TestClock.layer())`: This is the v4 test layer used by @effect/vitest. Prefer it.effect, which provides it automatically.
