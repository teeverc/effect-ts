# v3 → v4 API map: effect/ScheduleDecision

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/ScheduleDecision`

- `ScheduleDecision.Done` -> `Cause.Done`: ScheduleDecision was removed from the v4 public model. A Schedule.fromStep step recurs by returning [output, Duration] and terminates with Cause.done(output).

- `ScheduleDecision.ScheduleDecision` -> `none`: ScheduleDecision was removed from the v4 public model. A Schedule.fromStep step recurs by returning [output, Duration] and terminates with Cause.done(output).

- `ScheduleDecision.continue` -> `Effect.succeed([output, duration])`: ScheduleDecision was removed from the v4 public model. A Schedule.fromStep step recurs by returning [output, Duration] and terminates with Cause.done(output).

- `ScheduleDecision.continueWith` -> `Effect.succeed([output, duration])`: ScheduleDecision was removed from the v4 public model. A Schedule.fromStep step recurs by returning [output, Duration] and terminates with Cause.done(output).

- `ScheduleDecision.isContinue` -> `none`: ScheduleDecision was removed from the v4 public model. A Schedule.fromStep step recurs by returning [output, Duration] and terminates with Cause.done(output). Branch on the Pull result instead of inspecting a decision value.

- `ScheduleDecision.isDone` -> `Cause.isDone`: ScheduleDecision was removed from the v4 public model. A Schedule.fromStep step recurs by returning [output, Duration] and terminates with Cause.done(output).
