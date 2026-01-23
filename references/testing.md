# Testing (TestClock)

Use this guide when tests depend on time.

- `TestClock` lets you set or adjust the current time for tests.
- Advance time to trigger scheduled effects without waiting in real time.
- Use it to make time-based tests deterministic and fast.

## Example

```ts
import * as Duration from "effect/Duration"
import * as Effect from "effect/Effect"
import * as TestClock from "effect/TestClock"

const program = Effect.gen(function*() {
  yield* TestClock.adjust(Duration.seconds(5))
})
```
