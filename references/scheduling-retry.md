# Scheduling and Retry

Use this guide when you need repetition, backoff, or retry policies.

- A `Schedule` describes a recurring policy and can produce output values.
- Use `Effect.repeat` to repeat on success; use `Effect.schedule` to only follow the schedule without an initial run.
- Use `Effect.retry` to retry failures according to a schedule.
- Compose schedules for backoff, jitter, or rate limiting.
- Choose `repeat` for success-based recurrence and `retry` for failure-based recovery.

## Example

```ts
import * as Effect from "effect/Effect"
import { pipe } from "effect/Function"
import * as Ref from "effect/Ref"
import * as Schedule from "effect/Schedule"

const program = Effect.gen(function*() {
  const counter = yield* Ref.make(0)
  const attempt = pipe(
    Ref.updateAndGet(counter, (n) => n + 1),
    Effect.flatMap((n) =>
      n >= 3 ? Effect.succeed(n) : Effect.fail(new Error(`attempt ${n}`))
    )
  )

  // first run + 2 retries = 3 total attempts
  yield* attempt.pipe(
    Effect.retry(Schedule.recurs(2))
  )
})
```
