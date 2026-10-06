# v3 → v4 API map: effect/FiberId

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/FiberId`

- `FiberId.Composite` -> `none`: Composite FiberId values were removed; v4 uses a single numeric fiber id.

- `FiberId.FiberId` -> `number`: V4 represents a fiber identity as the numeric Fiber.id field.

- `FiberId.FiberIdTypeId` -> `none`: Fiber IDs are primitive numbers in v4 and have no type-id symbol.

- `FiberId.None` -> `undefined`: Optional interruptor IDs use undefined rather than a sentinel FiberId type.

- `FiberId.Runtime` -> `number`: Runtime fiber IDs are primitive numbers in v4.

- `FiberId.Single` -> `number | undefined`: Use a number, with undefined only where the old None case was meaningful.

- `FiberId.combine` -> `none`: Composite FiberId values were removed; v4 uses a single numeric fiber id.

- `FiberId.combineAll` -> `none`: Composite FiberId values were removed; v4 uses a single numeric fiber id.

- `FiberId.composite` -> `none`: Composite FiberId values were removed; v4 uses a single numeric fiber id.

- `FiberId.getOrElse` -> `fiberId ?? fallback`: Represent absence as undefined when migrating code that previously used FiberId.none.

- `FiberId.ids` -> `new Set([fiberId])`: A v4 fiber has one numeric id; composite-id flattening is no longer required.

- `FiberId.isComposite` -> `none`: Composite FiberId values do not exist in v4.

- `FiberId.isFiberId` -> `Number.isNumber`: Fiber IDs are primitive numbers in v4.

- `FiberId.isNone` -> `fiberId === undefined`: Use undefined for an absent optional interruptor id; there is no sentinel FiberId.none.

- `FiberId.isRuntime` -> `Number.isNumber`: Every v4 fiber id is a runtime numeric id.

- `FiberId.make` -> `id`: Use the numeric id directly; startTimeSeconds is no longer part of fiber identity.

- `FiberId.none` -> `undefined`: Optional interruptor IDs use undefined rather than a sentinel FiberId value.

- `FiberId.runtime` -> `id`: Use the numeric id directly; startTimeMillis is no longer part of fiber identity.

- `FiberId.threadName` -> `String(fiberId)`: There is no built-in thread-name formatter; format the numeric id at the presentation boundary.

- `FiberId.toSet` -> `new Set([fiberId])`: A v4 fiber has one numeric id, so composite-id flattening is unnecessary.

- `FiberId.unsafeMake` -> `none`: There is no public fiber-id allocator; obtain the current id with Effect.fiberId or from Fiber.id.
