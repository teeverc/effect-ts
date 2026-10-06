# Testing Time (TestClock) and Determinism

Use this guide when tests involve `Effect.sleep`, timeouts, schedules,
retries, caches with TTLs or anything else driven by `Clock`. For the test
runner (`@effect/vitest`), layers, fixtures, property tests and console capture
see `references/testing-stack.md`. Official examples:
`references/examples/09_testing/10_effect-tests.ts`,
`references/examples/09_testing/20_layer-tests.ts`.

## Mental model

- `TestClock` (from `effect/testing`) replaces the `Clock` service. Time starts
  at `0` (epoch) and only moves when you call `TestClock.adjust` or
  `TestClock.setTime`.
- `Effect.sleep` (and everything built on it: `timeout`, `Schedule`,
  `retry`, `repeat`, `delay`, TTL caches) semantically blocks until test time
  reaches the wake-up point. Fork the effect under test, move time, then
  observe.
- `it.effect` from `@effect/vitest` already provides `TestClock.layer()` and
  `TestConsole.layer`. `it.live` does not (real clock).
- Outside `@effect/vitest`, provide `TestClock.layer()` yourself.
- `TestClock` warns if a test sleeps without ever adjusting the clock
  (option `warningDelay` on `TestClock.layer({ warningDelay })`).

## API (`import { TestClock } from "effect/testing"`)

| API | Purpose |
| --- | --- |
| `TestClock.layer(options?)` | `Layer<TestClock>` installing the test clock as `Clock` |
| `TestClock.adjust(duration)` | advance time by a `Duration.Input` and run due sleepers in order |
| `TestClock.setTime(epochMillis)` | jump to an absolute timestamp |
| `TestClock.withLive(effect)` | run an effect with the real clock |
| `TestClock.testClockWith(f)` | access the `TestClock` service |
| `TestClock.make(options?)` | construct a test clock manually |

## Walkthrough: test a delay

```ts
import { assert, it } from "@effect/vitest"
import { Effect, Fiber } from "effect"
import { TestClock } from "effect/testing"

it.effect("completes after 5 minutes", () =>
  Effect.gen(function*() {
    const fiber = yield* Effect.forkChild(
      Effect.sleep("5 minutes").pipe(Effect.as("done" as const))
    )
    yield* TestClock.adjust("4 minutes")
    assert.isUndefined(fiber.pollUnsafe())              // still sleeping
    yield* TestClock.adjust("1 minute")
    assert.strictEqual(yield* Fiber.join(fiber), "done")
  }))
```

## Timeouts, retries and schedules

```ts
import { assert, it } from "@effect/vitest"
import { Effect, Exit, Fiber, Ref, Schedule } from "effect"
import { TestClock } from "effect/testing"

it.effect("timeout fires", () =>
  Effect.gen(function*() {
    const fiber = yield* Effect.sleep("5 minutes").pipe(
      Effect.timeout("1 minute"),
      Effect.forkChild
    )
    yield* TestClock.adjust("1 minute")
    const exit = yield* Fiber.await(fiber)
    assert.isTrue(Exit.isFailure(exit))
  }))

it.effect("retries with spaced schedule", () =>
  Effect.gen(function*() {
    const attempts = yield* Ref.make(0)
    const flaky = Ref.updateAndGet(attempts, (n) => n + 1).pipe(
      Effect.flatMap((n) => n < 3 ? Effect.fail("boom") : Effect.succeed(n))
    )
    const fiber = yield* flaky.pipe(
      Effect.retry(Schedule.spaced("10 seconds")),
      Effect.forkChild
    )
    yield* TestClock.adjust("10 seconds") // attempt 2
    yield* TestClock.adjust("10 seconds") // attempt 3 succeeds
    assert.strictEqual(yield* Fiber.join(fiber), 3)
  }))
```

## Without @effect/vitest

```ts
import { Effect, Fiber } from "effect"
import { TestClock } from "effect/testing"

const program = Effect.gen(function*() {
  const fiber = yield* Effect.forkChild(Effect.sleep("1 hour"))
  yield* TestClock.adjust("1 hour")
  yield* Fiber.join(fiber)
})

await Effect.runPromise(Effect.provide(program, TestClock.layer()))
```

## Other sources of non-determinism

- **Wall clock vs monotonic**: `Clock.currentTimeMillis` / `DateTime.now` read
  wall-clock test time; `Effect.timed`, `Effect.trackDuration` use
  `Clock.monotonicTimeNanos`. Both are driven by `TestClock`.
- **Randomness**: `Random.withSeed(seed)` for reproducible `Random.next*`.
- **Config**: `ConfigProvider.layer(ConfigProvider.fromUnknown({ PORT: 3000 }))`
  or `ConfigProvider.fromEnv({ env: { ... } })`.
- **Metrics**: give each test a fresh registry with
  `Effect.provideService(Metric.MetricRegistry, new Map())`.
- **Real time on purpose**: `it.live(...)` or `TestClock.withLive(effect)`.

## Pitfalls

- Not forking: `yield* Effect.sleep("1 hour")` in the test fiber blocks forever
  because nothing advances the clock (you'll see the TestClock warning, then a
  Vitest timeout).
- Use `Effect.forkChild` (child of the test fiber, auto-interrupted) rather than
  `Effect.forkDetach` in tests.
- `Date.now()` / `new Date()` bypass `TestClock`; use `Clock` or `DateTime.now`.
- Adjusting by less than the scheduled delay leaves the fiber suspended; check
  with `fiber.pollUnsafe()` or `Fiber.await` after the final adjust.

## v3 -> v4

- `TestClock` moved to `effect/testing` (`import { TestClock } from "effect/testing"`).
- `TestContext.TestContext`, `TestServices`, `TestLive`, `TestSized`, `TestConfig`
  are gone; `it.effect` provides `TestClock` + `TestConsole`, or provide
  `TestClock.layer()` directly. `TestServices.provideLive` -> `TestClock.withLive`.
- `Effect.fork` -> `Effect.forkChild`.
- See `references/migration/api-map/effect-TestClock.md`,
  `references/migration/api-map/effect-TestContext.md`,
  `references/migration/forking.md`.
