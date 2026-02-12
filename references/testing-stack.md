# Testing Stack (Beyond TestClock)

Use this guide when tests need more than time control.

## Mental model

- Test services are provided via `TestContext`.
- Override dependencies with `Effect.provideService` or layers.
- Keep tests deterministic by default, then opt into live dependencies explicitly.

## Patterns

- Provide `TestContext.TestContext` at test boundaries.
- Use `Effect.provideService` to inject deterministic test doubles.
- Prefer layer-based overrides for external dependencies (HTTP, DB, clock, random).
- Keep assertions on typed results (`Exit`, domain errors), not only string logs.

## Walkthrough: override a dependency in tests

```ts
import { Context, Effect, TestContext } from "effect"

interface ClockService {
  readonly now: Effect.Effect<number>
}
const ClockService = Context.Tag<ClockService>("ClockService")

const program = Effect.gen(function*() {
  const clock = yield* ClockService
  return yield* clock.now
})

const test = program.pipe(
  Effect.provideService(ClockService, { now: Effect.succeed(1234) }),
  Effect.provide(TestContext.TestContext)
)
```

## Pitfalls

- Forgetting to use `TestContext` in test environments.
- Leaking live services into tests unintentionally.
- Depending on wall-clock time or non-deterministic randomness when not required.
- Asserting only happy-path values and missing typed failure cases.

## Docs

- `https://effect.website/docs/testing/testclock/`
- `https://effect.website/docs/requirements-management/default-services/`
