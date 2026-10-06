# v3 → v4 API map: effect/FiberStatus

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/FiberStatus`

- `FiberStatus.Done` -> `Exit.Exit`: FiberStatus was removed; a defined fiber.pollUnsafe() result indicates completion and contains the Exit.

- `FiberStatus.FiberStatus` -> `Exit.Exit | undefined`: Use fiber.pollUnsafe(); undefined means incomplete and Exit means completed, with no running/suspended distinction.

- `FiberStatus.FiberStatusTypeId` -> `none`: FiberStatus and its type-id symbol were removed.

- `FiberStatus.Running` -> `none`: The public runtime no longer models running status as a value.

- `FiberStatus.Suspended` -> `none`: The public runtime no longer models suspended status as a value.

- `FiberStatus.isDone` -> `fiber.pollUnsafe() !== undefined`: Completion is observable by synchronously polling the Fiber.

- `FiberStatus.isFiberStatus` -> `none`: FiberStatus values no longer exist; inspect a Fiber with pollUnsafe instead.

- `FiberStatus.isRunning` -> `fiber.pollUnsafe() === undefined`: V4 only exposes incomplete versus completed; it does not distinguish running from suspended.

- `FiberStatus.isSuspended` -> `none`: The public runtime no longer exposes suspended status.

- `FiberStatus.running` -> `none`: FiberStatus constructors were removed; keep the Fiber and poll it instead.

- `FiberStatus.suspended` -> `none`: FiberStatus constructors and public suspended status were removed.
