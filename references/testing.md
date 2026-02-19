# Testing (TestClock)

Use this guide when tests depend on time.

## Mental model

- `TestClock` controls time in tests.
- Adjusting the clock triggers scheduled effects.

## Patterns

- Fork the effect under test, then adjust time.
- Provide `TestClock.layer()` from `effect/testing` to enable TestClock.

## Walkthrough: test a delay

```ts
import * as Effect from "effect/Effect"
import * as Fiber from "effect/Fiber"
import { TestClock } from "effect/testing"

const program = Effect.gen(function*() {
  yield* Effect.sleep("5 minutes")
  return "done"
})

const test = Effect.gen(function*() {
  const fiber = yield* Effect.forkChild(program)
  yield* TestClock.adjust("5 minutes")
  return yield* Fiber.join(fiber)
}).pipe(Effect.provide(TestClock.layer()))
```

## Pitfalls

- Forgetting to provide `TestClock.layer()`.
- Adjusting the clock without forking the effect.
