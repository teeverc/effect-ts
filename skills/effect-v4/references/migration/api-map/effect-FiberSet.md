# v3 → v4 API map: effect/FiberSet

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/FiberSet`

- `FiberSet.FiberSet` -> `FiberSet.FiberSet`: Retained; contained runtime fibers now use the unified Fiber type.

- `FiberSet.TypeId` -> `FiberSet.isFiberSet`: The type-id symbol is private in v4; use the public guard.

- `FiberSet.unsafeAdd` -> `FiberSet.addUnsafe`: Renamed to put the Unsafe suffix last; the interruptAs option was removed because IDs are now numeric runtime details.
