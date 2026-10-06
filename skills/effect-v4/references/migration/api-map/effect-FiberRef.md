# v3 → v4 API map: effect/FiberRef

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/FiberRef`

- `FiberRef.FiberRef` -> `Context.Reference`: Fiber-local values and services share Context.Reference in v4; references have a defaultValue and no fork/join patching.

- `FiberRef.FiberRefTypeId` -> `Context.isReference`: Use the public Context.Reference guard instead of a FiberRef type-id symbol.

- `FiberRef.FiberRefUnify` -> `none`: Context.Reference is a service key and does not require the old FiberRef Effect-unification helper.

- `FiberRef.FiberRefUnifyIgnore` -> `none`: Context.Reference is a service key and does not require the old FiberRef Effect-unification helper.

- `FiberRef.Variance` -> `Context.Reference`: The FiberRef-specific variance interface was removed with FiberRef.

- `FiberRef.currentConcurrency` -> `none`: Inherited concurrency was removed; pass concurrency explicitly to each v4 combinator that supports it.

- `FiberRef.currentContext` -> `Effect.context`: Fiber services are stored directly in Context; use Effect.context to read them and Effect.provideContext to override them.

- `FiberRef.currentLogAnnotations` -> `References.CurrentLogAnnotations`: Built-in FiberRefs are now Context.Reference values; yield the reference or provide it with Effect.provideService.

- `FiberRef.currentLogLevel` -> `References.CurrentLogLevel`: Built-in FiberRefs are now Context.Reference values; yield the reference or provide it with Effect.provideService.

- `FiberRef.currentLogSpan` -> `References.CurrentLogSpans`: Renamed and represented as a Context.Reference containing a readonly span array.

- `FiberRef.currentLoggers` -> `References.CurrentLoggers`: Built-in FiberRefs are now Context.Reference values; yield the reference or provide it with Effect.provideService.

- `FiberRef.currentMaxOpsBeforeYield` -> `Scheduler.MaxOpsBeforeYield`: The scheduler setting is now a Context.Reference; yield it or provide it with Effect.provideService.

- `FiberRef.currentMetricLabels` -> `Metric.CurrentMetricAttributes`: Metric labels became metric attributes stored in a Context.Reference.

- `FiberRef.currentMinimumLogLevel` -> `References.MinimumLogLevel`: Built-in FiberRefs are now Context.Reference values; yield the reference or provide it with Effect.provideService.

- `FiberRef.currentRequestBatchingEnabled` -> `none`: The request batching FiberRef was removed; batching is defined by the v4 RequestResolver runAll implementation.

- `FiberRef.currentRequestCache` -> `RequestResolver.withCache`: The ambient request cache was removed; wrap a RequestResolver with an explicit bounded cache.

- `FiberRef.currentRequestCacheEnabled` -> `RequestResolver.withCache`: There is no ambient cache toggle; choose an explicitly cached or uncached RequestResolver.

- `FiberRef.currentRuntimeFlags` -> `none`: RuntimeFlags and their FiberRef were removed; use specific v4 runtime options such as interruptibility and scheduler settings.

- `FiberRef.currentScheduler` -> `Scheduler.Scheduler`: The scheduler is now a Context.Reference; yield it or provide it with Effect.provideService.

- `FiberRef.currentSchedulingPriority` -> `none`: The ambient scheduling-priority FiberRef was removed; use explicit scheduler operations where priority is needed.

- `FiberRef.currentSupervisor` -> `none`: The Supervisor and ambient supervisor FiberRef APIs were removed; track fibers explicitly with FiberSet or FiberMap.

- `FiberRef.currentTracerEnabled` -> `References.TracerEnabled`: Built-in FiberRefs are now Context.Reference values; yield the reference or provide it with Effect.provideService.

- `FiberRef.currentTracerSpanAnnotations` -> `References.TracerSpanAnnotations`: Built-in FiberRefs are now Context.Reference values; yield the reference or provide it with Effect.provideService.

- `FiberRef.currentTracerSpanLinks` -> `References.TracerSpanLinks`: Built-in FiberRefs are now Context.Reference values; yield the reference or provide it with Effect.provideService.

- `FiberRef.currentTracerTimingEnabled` -> `References.TracerTimingEnabled`: Built-in FiberRefs are now Context.Reference values; yield the reference or provide it with Effect.provideService.

- `FiberRef.delete` -> `Effect.provideService`: Context.Reference has no in-place delete; scope the default or desired value around the target Effect.

- `FiberRef.get` -> `reference`: Context.Reference is yieldable as a service; yield it directly to read the current value.

- `FiberRef.getAndUpdateSome` -> `Ref.getAndUpdateSome`: Use Ref for mutable state; for fiber-local configuration compute the value first and scope it with Effect.provideService.

- `FiberRef.getWith` -> `Effect.flatMap(reference, f)`: Yield or flatMap the Context.Reference directly.

- `FiberRef.interruptedCause` -> `none`: The pending interruption cause is no longer exposed as public fiber-local state; inspect completed failure Causes from Fiber.await.

- `FiberRef.make` -> `Context.Reference`: Define a stable Context.Reference key with defaultValue; custom fork and join behavior is not supported.

- `FiberRef.makeContext` -> `Context.Reference`: Define a Context.Reference whose defaultValue returns the Context; custom context diffing is no longer required.

- `FiberRef.makeRuntimeFlags` -> `none`: RuntimeFlags and specialized FiberRef constructors were removed; migrate each flag to its explicit v4 runtime option.

- `FiberRef.makeWith` -> `Context.Reference`: Use the lazy defaultValue option on a stable Context.Reference key.

- `FiberRef.modify` -> `Ref.modify`: Use Ref for mutable state; Context.Reference updates are scoped with Effect.provideService rather than mutated in place.

- `FiberRef.modifySome` -> `Ref.modifySome`: Use Ref for mutable state; Context.Reference updates are scoped with Effect.provideService rather than mutated in place.

- `FiberRef.reset` -> `Effect.provideService`: Context.Reference has no in-place reset; scope its default value around the target Effect.

- `FiberRef.set` -> `Effect.provideService`: Context.Reference values are overridden for an Effect scope instead of mutating the current fiber.

- `FiberRef.unhandledErrorLogLevel` -> `References.UnhandledLogLevel`: Renamed and represented as a Context.Reference using Severity | undefined instead of Option\<LogLevel\>.

- `FiberRef.unsafeMake` -> `Context.Reference`: Context.Reference construction is synchronous; provide a stable identifier and defaultValue.

- `FiberRef.unsafeMakeContext` -> `Context.Reference`: Define a Context.Reference whose defaultValue returns the Context; there is no specialized unsafe constructor.

- `FiberRef.unsafeMakeHashSet` -> `Context.Reference`: Define a normal Context.Reference with a readonly set default; specialized differ constructors were removed.

- `FiberRef.unsafeMakePatch` -> `Context.Reference`: Define a normal Context.Reference; custom Differ, fork patches, and join behavior are not supported in v4.

- `FiberRef.unsafeMakeSupervisor` -> `none`: Supervisor and FiberRef were removed; track managed fibers explicitly with FiberSet or FiberMap.

- `FiberRef.update` -> `Ref.update`: Use Ref for mutable state; for fiber-local configuration compute a value and scope it with Effect.provideService.

- `FiberRef.updateSome` -> `Ref.updateSome`: Use Ref for mutable state; for fiber-local configuration compute a value and scope it with Effect.provideService.

- `FiberRef.updateSomeAndGet` -> `Ref.updateSomeAndGet`: Use Ref for mutable state; for fiber-local configuration compute a value and scope it with Effect.provideService.

- `FiberRef.versionMismatchErrorLogLevel` -> `none`: The version-mismatch logging FiberRef was removed and no public v4 Context.Reference replaces it.
