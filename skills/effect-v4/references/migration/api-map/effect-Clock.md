# v3 → v4 API map: effect/Clock

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Clock`

- `Clock.CancelToken` -> `none`: The public clock scheduler and cancellation-token protocol were removed. Use Effect.sleep for delays and Effect interruption or Fiber.interrupt for cancellation.

- `Clock.Clock` -> `Clock.Clock`: The service interface remains, but unsafeCurrentTimeMillis and unsafeCurrentTimeNanos were renamed to currentTimeMillisUnsafe and currentTimeNanosUnsafe, the public type-id field was removed, and custom implementations must add monotonicTimeNanosUnsafe plus monotonicTimeNanos for elapsed-time measurement.

- `Clock.ClockScheduler` -> `none`: The low-level clock scheduler is no longer public. Express scheduling with Effect.sleep and cancel the running fiber through normal Effect interruption.

- `Clock.ClockTypeId` -> `none`: The Clock type-id is private in v4. Use the Clock.Clock Context.Reference to access, provide, or identify the clock service.

- `Clock.Task` -> `none`: The low-level clock task alias was removed with ClockScheduler. Model delayed work as an Effect and run or fork it after Effect.sleep.

- `Clock.make` -> `Layer.succeed(Clock.Clock, clock)`: The Clock constructor was removed. Implement the v4 Clock interface as a plain service value and provide it through Clock.Clock.
