# Concurrency Advanced (Interruption, Supervision, References)

Use this guide when coordinating fibers beyond simple forking.

## Mental model

- Interruption is cooperative; attach cleanup with `Effect.onInterrupt`.
- Supervisors and scopes keep child fibers bound to a lifetime.
- Fiber-local state is handled with `ServiceMap.Reference` values (exported from `References`).

## Patterns

- Use `Effect.forkScoped` to tie a fiber to a scope.
- Use `Fiber.interrupt` and `Fiber.join` to manage lifetimes.
- Read references with `yield* References.*`.
- Scope updates with `Effect.provideService`.

## Walkthrough: fiber-local state

```ts
import { Effect, References } from "effect"

const program = Effect.provideService(
  Effect.gen(function* () {
    const level = yield* References.CurrentLogLevel
    return level
  }),
  References.CurrentLogLevel,
  "Debug"
)
```

## Pitfalls

- Detaching fibers without a scope.
- Assuming interruption is preemptive.
