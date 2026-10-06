# v3 → v4 API map: effect/Queue

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Queue`

- `Queue.BackingQueue` -> `none`: Custom backing queues were removed by the v4 Queue rewrite; use Queue.make and its built-in capacity and strategy options.

- `Queue.BackingQueueTypeId` -> `none`: BackingQueue and its public type id were removed.

- `Queue.BaseQueue` -> `Queue.Enqueue | Queue.Dequeue`: The shared BaseQueue interface was removed; accept the required enqueue or dequeue capability and call Queue operations explicitly.

- `Queue.Dequeue` -> `Queue.Dequeue`: The model remains and gains an error parameter, but is no longer an Effect subtype; use Queue.take explicitly.

- `Queue.DequeueTypeId` -> `Queue.isDequeue`: The dequeue type id is internal in v4; use Queue.isDequeue for runtime refinement.

- `Queue.DequeueUnify` -> `none`: Queue.Dequeue is no longer an Effect subtype, so its Effect unification helper was removed.

- `Queue.DequeueUnifyIgnore` -> `none`: Queue.Dequeue is no longer an Effect subtype, so its Effect unification ignore marker was removed.

- `Queue.Enqueue` -> `Queue.Enqueue`: The write-side model remains, gains an error parameter, and is operated through Queue.offer and related functions.

- `Queue.EnqueueTypeId` -> `Queue.isEnqueue`: The enqueue type id is internal in v4; use Queue.isEnqueue for runtime refinement.

- `Queue.Queue` -> `Queue.Queue`: The model remains, gains an error parameter and completion signaling, and is no longer an Effect subtype; use Queue.take explicitly.

- `Queue.Queue.BackingQueueVariance` -> `none`: BackingQueue and its variance marker were removed by the v4 Queue rewrite.

- `Queue.Queue.DequeueVariance` -> `Queue.Dequeue.Variance`: The read-side variance marker moved under the Queue.Dequeue namespace and now includes the error type.

- `Queue.Queue.EnqueueVariance` -> `Queue.Enqueue.Variance`: The write-side variance marker moved under the Queue.Enqueue namespace and now includes the error type.

- `Queue.Queue.StrategyVariance` -> `none`: Public Strategy values and their variance marker were removed; select a string strategy when constructing the Queue.

- `Queue.QueueStrategyTypeId` -> `none`: Public Strategy values and their type id were removed.

- `Queue.QueueUnify` -> `none`: Queue is no longer an Effect subtype, so its Effect unification helper was removed; call Queue.take explicitly.

- `Queue.QueueUnifyIgnore` -> `none`: Queue is no longer an Effect subtype, so its Effect unification ignore marker was removed.

- `Queue.Strategy` -> `"suspend" | "dropping" | "sliding"`: The pluggable Strategy interface was removed; choose one of the built-in strategy strings in Queue.make.

- `Queue.awaitShutdown` -> `Queue.await`: Queue completion now includes normal end and failure; Queue.await waits for Done and propagates non-Done terminal causes.

- `Queue.backPressureStrategy` -> `Queue.make({ strategy: "suspend" })`: Strategies are now constructor options rather than public Strategy values; suspend is the default.

- `Queue.capacity` -> `queue.capacity`: Capacity is now a property on Queue.Enqueue and Queue.Dequeue rather than a module function.

- `Queue.droppingStrategy` -> `Queue.make({ strategy: "dropping" })`: Strategies are now constructor options rather than public Strategy values; Queue.dropping is the bounded convenience constructor.

- `Queue.isEmpty` -> `Effect.map(Queue.size(self), (size) => size === 0)`: The dedicated helper was removed; derive emptiness from Queue.size.

- `Queue.isShutdown` -> `queue.state._tag === "Done"`: The dedicated helper was removed; inspect the public queue lifecycle state. Done includes normal completion and failure, not only explicit shutdown.

- `Queue.slidingStrategy` -> `Queue.make({ strategy: "sliding" })`: Strategies are now constructor options rather than public Strategy values; Queue.sliding is the bounded convenience constructor.

- `Queue.takeUpTo` -> `Queue.poll`: No direct bounded batch helper remains; repeatedly call non-blocking Queue.poll up to the limit, or use Queue.clear when taking every buffered value is acceptable.

- `Queue.unsafeOffer` -> `Queue.offerUnsafe`: The unsafe suffix moved to the end.
