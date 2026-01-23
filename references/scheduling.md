# Scheduling (Schedule, Repeat, Effect.schedule)

Use this guide when modeling repetition or time-based policies.

- A `Schedule` describes a recurring policy for running an effect.
- A schedule consumes inputs and can produce output values across its recurrences.
- `Effect.repeat` runs the effect once and then follows the schedule for further runs.
- `Effect.schedule` skips the initial run and follows only the schedule.
- Use schedules for repetition and retry policies.
