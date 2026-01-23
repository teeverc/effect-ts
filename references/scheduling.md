# Scheduling (Schedule, Repeat, Effect.schedule)

Use this guide when modeling repetition or time-based policies.

- A `Schedule` describes a recurring policy for running an effect.
- A schedule consumes inputs and can produce output values across its recurrences.
- `Effect.repeat` runs the effect once and then follows the schedule for further runs.
- `Effect.schedule` skips the initial run and follows only the schedule.
- Use schedules for repetition and retry policies.

## Example

```ts
import * as Effect from "effect/Effect"
import * as Schedule from "effect/Schedule"

const program = Effect.succeed("tick").pipe(
  Effect.repeat(Schedule.recurs(3))
)
```
