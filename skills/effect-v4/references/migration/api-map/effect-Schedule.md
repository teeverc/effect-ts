# v3 → v4 API map: effect/Schedule

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Schedule`

- `Schedule.CurrentIterationMetadata` -> `Schedule.CurrentMetadata`: The Context.Reference was renamed and now provides Schedule.Metadata with input, output, attempt, duration, and elapsed timing fields.

- `Schedule.IterationMetadata` -> `Schedule.Metadata`: The metadata model now includes duration and uses attempt instead of recurrence; elapsed fields are millisecond numbers.

- `Schedule.Schedule` -> `Schedule.Schedule`: The model remains but now has Schedule\<Output, Input, Error, Env\>; its public initial/step fields were replaced by Schedule.toStep and fromStep.

- `Schedule.Schedule.DriverVariance` -> `none`: ScheduleDriver was removed in v4, so its variance marker has no replacement. Use the Schedule type parameters or the function returned by Schedule.toStepWithSleep.

- `Schedule.Schedule.Variance` -> `Schedule.Schedule.Variance`: The variance marker remains and now tracks Output, Input, Error, and Env through the private Schedule TypeId.

- `Schedule.ScheduleDriver` -> `Schedule.toStepWithSleep`: ScheduleDriver was removed. The acquired step function provides manual next calls with automatic sleeping; Schedule.toStep exposes raw delays.

- `Schedule.ScheduleDriverTypeId` -> `none`: ScheduleDriver and its public type id were removed. Use the step function returned by Schedule.toStepWithSleep.

- `Schedule.ScheduleTypeId` -> `none`: The Schedule type id is private in v4. Use Schedule.isSchedule to narrow unknown values.

- `Schedule.addDelayEffect` -> `Schedule.addDelay`: The v4 function is effectful by default and its callback receives full Schedule.Metadata; read metadata.output when only the prior output is needed.

- `Schedule.andThen` -> `Schedule.concat`: The sequencing combinator was renamed to Schedule.concat.

- `Schedule.andThenEither` -> `Schedule.concatResult`: Sequential phase tagging now uses Result: self outputs are Result.fail and the following schedule outputs are Result.succeed.

- `Schedule.as` -> `Schedule.map`: Map the metadata to the constant output; Schedule.map accepts either a plain value or an Effect.

- `Schedule.asVoid` -> `Schedule.map`: Map every output to undefined.

- `Schedule.bothInOut` -> `none`: There is no direct v4 combinator preserving this output shape. Rebuild it with Schedule.fromStep and Schedule.toStep; schedule steps now return [output, Duration] and terminate with Cause.done.

- `Schedule.check` -> `Schedule.while`: Continue while a predicate over metadata.input and metadata.output returns true.

- `Schedule.checkEffect` -> `Schedule.while`: Schedule.while accepts an effectful metadata predicate in v4.

- `Schedule.collectAllInputs` -> `none`: This stateful collection combinator was removed during the v4 Schedule simplification. Rebuild it with Schedule.fromStep and Schedule.toStep, keeping accumulation state inside the acquired step closure.

- `Schedule.collectAllOutputs` -> `none`: This stateful collection combinator was removed during the v4 Schedule simplification. Rebuild it with Schedule.fromStep and Schedule.toStep, keeping accumulation state inside the acquired step closure.

- `Schedule.collectUntil` -> `none`: This stateful collection combinator was removed during the v4 Schedule simplification. Rebuild it with Schedule.fromStep and Schedule.toStep, keeping accumulation state inside the acquired step closure.

- `Schedule.collectUntilEffect` -> `none`: This stateful collection combinator was removed during the v4 Schedule simplification. Rebuild it with Schedule.fromStep and Schedule.toStep, keeping accumulation state inside the acquired step closure.

- `Schedule.collectWhile` -> `none`: This stateful collection combinator was removed during the v4 Schedule simplification. Rebuild it with Schedule.fromStep and Schedule.toStep, keeping accumulation state inside the acquired step closure.

- `Schedule.collectWhileEffect` -> `none`: This stateful collection combinator was removed during the v4 Schedule simplification. Rebuild it with Schedule.fromStep and Schedule.toStep, keeping accumulation state inside the acquired step closure.

- `Schedule.compose` -> `none`: There is no direct v4 combinator preserving this output shape. Rebuild it with Schedule.fromStep and Schedule.toStep; schedule steps now return [output, Duration] and terminate with Cause.done.

- `Schedule.count` -> `Schedule.forever`: The forever schedule outputs the zero-based recurrence count.

- `Schedule.dayOfMonth` -> `Schedule.cron`: Express the calendar constraint as a cron expression, for example `0 0 <day> * *`, and map its Duration output if a numeric output is required.

- `Schedule.dayOfWeek` -> `Schedule.cron`: Express the weekday constraint as a cron expression, for example `0 0 * * <day>`, and map its Duration output if a numeric output is required.

- `Schedule.delayed` -> `Schedule.modifyDelay`: Return Effect.succeed(f(metadata.duration)); delay transformations are effectful and receive full metadata in v4.

- `Schedule.delayedEffect` -> `Schedule.modifyDelay`: The v4 delay modifier is effectful by default and receives full Schedule.Metadata.

- `Schedule.delayedSchedule` -> `Schedule.modifyDelay`: Replace each delay with metadata.output, converting that Duration output through Effect.succeed.

- `Schedule.delays` -> `Schedule.map`: Map each decision to metadata.duration to expose the selected recurrence delay.

- `Schedule.driver` -> `Schedule.toStepWithSleep`: Acquire the sleeping step function and call it for each input; use Schedule.toStep when delay handling must remain manual.

- `Schedule.either` -> `Schedule.min`: Use Schedule.min for fastest-delay composition. It outputs the selected Duration rather than a tuple of both outputs.

- `Schedule.eitherWith` -> `Schedule.min`: Schedule.min implements the standard fastest-delay composition; custom interval merging requires a Schedule.fromStep implementation.

- `Schedule.elapsed` -> `Schedule.map`: Map metadata.elapsed through Duration.millis.

- `Schedule.ensuring` -> `Schedule.during`: Use the duration-bounded v4 schedule constructor.

- `Schedule.fromDelay` -> `Schedule.duration`: The duration constructor recurs once after the supplied delay.

- `Schedule.fromDelays` -> `Schedule.duration + Schedule.concat`: Build one Schedule.duration per delay and sequence them with Schedule.concat.

- `Schedule.fromFunction` -> `Schedule.identity + Schedule.map`: Start with Schedule.identity\<A\>() and map metadata.input through the function.

- `Schedule.hourOfDay` -> `Schedule.cron`: Express the hour constraint as a cron expression such as `0 <hour> * * *`.

- `Schedule.intersect` -> `Schedule.max`: Use Schedule.max for slowest-delay composition. It outputs the selected Duration rather than a tuple of both outputs.

- `Schedule.intersectWith` -> `Schedule.max`: Schedule.max implements the standard slowest-delay composition; custom interval merging requires a Schedule.fromStep implementation.

- `Schedule.jitteredWith` -> `Schedule.modifyDelay`: For custom bounds, scale metadata.duration using Random.next inside the effectful delay callback; Schedule.jittered supplies the fixed v4 0.8-1.2 range.

- `Schedule.linear` -> `Schedule.forever + Schedule.map + Schedule.modifyDelay`: Map the recurrence attempt to the linearly increasing Duration, then use that output as the recurrence delay.

- `Schedule.makeWithState` -> `Schedule.fromStep`: Move mutable state into the acquired step closure; return [output, Duration] for recurrence and Cause.done(output) for termination.

- `Schedule.mapBoth` -> `Schedule.fromStep + Schedule.toStep`: Input transformation is no longer a standalone combinator. Wrap Schedule.toStep(self) with Schedule.fromStep and transform the input before invoking the underlying step.

- `Schedule.mapBothEffect` -> `Schedule.fromStep + Schedule.toStep`: Input transformation is no longer a standalone combinator. Wrap Schedule.toStep(self) with Schedule.fromStep and transform the input before invoking the underlying step. Apply the effectful output mapping to the returned tuple.

- `Schedule.mapEffect` -> `Schedule.map`: Schedule.map accepts an Effect result and receives full Schedule.Metadata.

- `Schedule.mapInput` -> `Schedule.fromStep + Schedule.toStep`: Input transformation is no longer a standalone combinator. Wrap Schedule.toStep(self) with Schedule.fromStep and transform the input before invoking the underlying step.

- `Schedule.mapInputContext` -> `Schedule.fromStep + Effect.provide`: Provide the transformed service context to both Schedule.toStep acquisition and each returned step Effect.

- `Schedule.mapInputEffect` -> `Schedule.fromStep + Schedule.toStep`: Input transformation is no longer a standalone combinator. Wrap Schedule.toStep(self) with Schedule.fromStep and transform the input before invoking the underlying step. Evaluate the input mapping Effect before the underlying step.

- `Schedule.minuteOfHour` -> `Schedule.cron`: Express the minute constraint as a cron expression such as `<minute> * * * *`.

- `Schedule.modifyDelayEffect` -> `Schedule.modifyDelay`: The v4 delay modifier is effectful by default and receives full Schedule.Metadata.

- `Schedule.onDecision` -> `Schedule.tap`: Use Schedule.tap for effects on recurrence metadata. To also observe final completion, wrap Schedule.toStep with Pull.matchEffect in Schedule.fromStep.

- `Schedule.provideContext` -> `Schedule.fromStep + Effect.provide`: Provide the Context to both Schedule.toStep acquisition and each Effect returned by the acquired step.

- `Schedule.provideService` -> `Schedule.fromStep + Effect.provideService`: Provide the service to both Schedule.toStep acquisition and each Effect returned by the acquired step.

- `Schedule.recurUntil` -> `Schedule.identity + Schedule.while`: Continue while the predicate over metadata.input is false.

- `Schedule.recurUntilEffect` -> `Schedule.identity + Schedule.while`: Continue while the effectful predicate over metadata.input is false.

- `Schedule.recurUntilOption` -> `Schedule.fromStep`: Use a custom step to evaluate the Option-producing function, emit Option.none while recurring, and terminate with the first Option.some result.

- `Schedule.recurUpTo` -> `Schedule.during`: Use the duration-bounded schedule constructor.

- `Schedule.recurWhile` -> `Schedule.identity + Schedule.while`: Continue while the predicate over metadata.input is true.

- `Schedule.recurWhileEffect` -> `Schedule.identity + Schedule.while`: Continue while the effectful predicate over metadata.input is true.

- `Schedule.reduce` -> `none`: This stateful collection combinator was removed during the v4 Schedule simplification. Rebuild it with Schedule.fromStep and Schedule.toStep, keeping accumulation state inside the acquired step closure.

- `Schedule.reduceEffect` -> `none`: This stateful collection combinator was removed during the v4 Schedule simplification. Rebuild it with Schedule.fromStep and Schedule.toStep, keeping accumulation state inside the acquired step closure.

- `Schedule.repeatForever` -> `Schedule.forever`: The infinite zero-delay counter schedule was renamed.

- `Schedule.repetitions` -> `Schedule.map`: Map metadata.attempt to the required recurrence count, adjusting by one where the v3 zero-based value is expected.

- `Schedule.resetAfter` -> `none`: Automatic schedule reset was removed. Wrap Schedule.toStep(self) with Schedule.fromStep and reacquire the inner step when the reset condition is met.

- `Schedule.resetWhen` -> `none`: Automatic schedule reset was removed. Wrap Schedule.toStep(self) with Schedule.fromStep and reacquire the inner step when the reset condition is met.

- `Schedule.run` -> `Schedule.toStep`: Acquire the step and traverse inputs manually, supplying each timestamp and collecting successful outputs until Cause.done.

- `Schedule.secondOfMinute` -> `Schedule.cron`: Use the six-field cron form to express a seconds constraint, for example `<second> * * * * *`.

- `Schedule.stop` -> `Schedule.fromStep`: Create a step that immediately returns Cause.done(undefined).

- `Schedule.succeed` -> `Schedule.forever + Schedule.map`: Map every recurrence to the constant value.

- `Schedule.sync` -> `Schedule.forever + Schedule.map`: Map every recurrence by lazily evaluating the thunk.

- `Schedule.tapInput` -> `Schedule.tap`: Use the unified tap callback and read metadata.input.

- `Schedule.tapOutput` -> `Schedule.tap`: Use the unified tap callback and read metadata.output.

- `Schedule.unfold` -> `Schedule.fromStep`: Keep the evolving value inside the acquired step closure and emit each value with the desired Duration.

- `Schedule.union` -> `Schedule.min`: Use Schedule.min for fastest-delay composition. It outputs the selected Duration rather than both schedule outputs.

- `Schedule.unionWith` -> `Schedule.min`: Schedule.min covers the standard union behavior; a custom interval merge requires Schedule.fromStep.

- `Schedule.untilInput` -> `Schedule.while`: Continue while the predicate over metadata.input is false.

- `Schedule.untilInputEffect` -> `Schedule.while`: Continue while the effectful predicate over metadata.input is false.

- `Schedule.untilOutput` -> `Schedule.while`: Continue while the predicate over metadata.output is false.

- `Schedule.untilOutputEffect` -> `Schedule.while`: Continue while the effectful predicate over metadata.output is false.

- `Schedule.whileInput` -> `Schedule.while`: Continue while the predicate over metadata.input is true.

- `Schedule.whileInputEffect` -> `Schedule.while`: Continue while the effectful predicate over metadata.input is true.

- `Schedule.whileOutput` -> `Schedule.while`: Continue while the predicate over metadata.output is true.

- `Schedule.whileOutputEffect` -> `Schedule.while`: Continue while the effectful predicate over metadata.output is true.

- `Schedule.zipLeft` -> `none`: There is no direct v4 combinator preserving this output shape. Rebuild it with Schedule.fromStep and Schedule.toStep; schedule steps now return [output, Duration] and terminate with Cause.done.

- `Schedule.zipRight` -> `none`: There is no direct v4 combinator preserving this output shape. Rebuild it with Schedule.fromStep and Schedule.toStep; schedule steps now return [output, Duration] and terminate with Cause.done.

- `Schedule.zipWith` -> `none`: There is no direct v4 combinator preserving this output shape. Rebuild it with Schedule.fromStep and Schedule.toStep; schedule steps now return [output, Duration] and terminate with Cause.done.
