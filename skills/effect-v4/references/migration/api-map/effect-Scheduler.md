# v3 → v4 API map: effect/Scheduler

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Scheduler`

- `Scheduler.ControlledScheduler` -> `none`: No public step-controlled scheduler remains; implement Scheduler and SchedulerDispatcher for exact controlled stepping.

- `Scheduler.MixedScheduler` -> `Scheduler.MixedScheduler`: The class remains with a redesigned constructor and makeDispatcher-based task API.

- `Scheduler.PriorityBuckets` -> `none`: Priority buckets are now an internal Scheduler implementation detail.

- `Scheduler.Scheduler` -> `Scheduler.Scheduler`: The interface remains but dispatch moved to SchedulerDispatcher returned by makeDispatcher.

- `Scheduler.SchedulerRunner` -> `Scheduler.SchedulerDispatcher`: Task scheduling and flushing moved to the dispatcher returned by Scheduler.makeDispatcher.

- `Scheduler.SyncScheduler` -> `new Scheduler.MixedScheduler("sync")`: Use a synchronous MixedScheduler and its dispatcher; call flush when directly driving queued tasks.

- `Scheduler.Task` -> `() => void`: The named alias was removed; dispatcher APIs inline the task callback type.

- `Scheduler.defaultScheduler` -> `Scheduler.Scheduler`: The default scheduler is now a Context.Reference; yield it to read or provide it to override the current scheduler.

- `Scheduler.defaultShouldYield` -> `Scheduler.MixedScheduler#shouldYield`: The standalone function was removed; yielding is implemented by each Scheduler instance.

- `Scheduler.make` -> `none`: Implement the redesigned Scheduler interface and return task dispatch through makeDispatcher.

- `Scheduler.makeBatched` -> `new Scheduler.MixedScheduler("async", schedule)`: Pass a cancellable scheduling function; the dispatcher performs priority batching.

- `Scheduler.makeMatrix` -> `none`: Matrix routing was removed; implement routing in a custom Scheduler and SchedulerDispatcher if still required.

- `Scheduler.timer` -> `Effect.delay`: Use Effect delay or sleep for effect timing; implement a custom dispatcher for exact per-task scheduler timing.

- `Scheduler.timerBatched` -> `new Scheduler.MixedScheduler("async", scheduleWithTimer)`: Use a setTimeout-based cancellable scheduling function; the dispatcher batches queued tasks.
