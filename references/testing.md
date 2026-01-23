# Testing (TestClock)

Use this guide when tests depend on time.

- `TestClock` lets you set or adjust the current time for tests.
- Advance time to trigger scheduled effects without waiting in real time.
- Use it to make time-based tests deterministic and fast.

## Example
```ts
import * as Duration from "effect/Duration"
import * as Effect from "effect/Effect"
import * as Fiber from "effect/Fiber"
import * as TestClock from "effect/TestClock"
import * as TestContext from "effect/TestContext"

const program = Effect.gen(function*() {
  const fiber = yield* Effect.fork(Effect.sleep(Duration.millis(100)))
  yield* TestClock.adjust(Duration.millis(100))
  yield* Fiber.join(fiber)
})

Effect.runPromise(program.pipe(Effect.provide(TestContext.TestContext)))
```
