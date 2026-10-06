# v3 → v4 API map: effect/Micro

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Micro`

- `Micro.All.IsDiscard` -> `Effect.All.IsDiscard`: Type-level helper moved to the Effect.All namespace.

- `Micro.All.MicroAny` -> `Effect.All.EffectAny`: Renamed: MicroAny becomes EffectAny in the Effect.All namespace.

- `Micro.All.Return` -> `Effect.All.Return`: Type-level helper moved to the Effect.All namespace.

- `Micro.CurrentConcurrency` -> `none`: Removed in v4 (no fiber-wide concurrency reference). Pass a { concurrency } option directly to the operations that fan out, e.g. Effect.all or Effect.forEach.

- `Micro.CurrentScheduler` -> `References.Scheduler`: The scheduler reference lives in effect/References (also exported from effect/Scheduler as Scheduler.Scheduler). Override it with Effect.provideService/Effect.updateService.

- `Micro.Do` -> `Effect.Do`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.Error` -> `Data.Error`: The yieldable error base class constructor is Data.Error from effect/Data in v4.

- `Micro.MaxOpsBeforeYield` -> `References.MaxOpsBeforeYield`: The reference lives in effect/References (also exported from effect/Scheduler). Override it with Effect.updateService.

- `Micro.Micro` -> `Effect.Effect`: The Micro\<A, E, R\> type is Effect.Effect\<A, E, R\> in v4; the v4 Effect runtime is itself lightweight.

- `Micro.Micro.Context` -> `Effect.Services`: Type extractor renamed: Micro.Context\<T\> becomes Effect.Services\<T\> in v4.

- `Micro.Micro.Error` -> `Effect.Error`: Type extractor: Micro.Error\<T\> becomes Effect.Error\<T\> in v4.

- `Micro.Micro.Success` -> `Effect.Success`: Type extractor: Micro.Success\<T\> becomes Effect.Success\<T\> in v4.

- `Micro.MicroCause` -> `Cause.Cause`: MicroCause\<E\> becomes Cause.Cause\<E\>. Note v4 Cause holds a list of failure reasons (Fail | Die | Interrupt) rather than being a single tagged variant.

- `Micro.MicroCause.Die` -> `Cause.Die`: The Die variant is a Reason in v4: Cause.Die from effect/Cause.

- `Micro.MicroCause.Error` -> `Cause.Cause.Error`: Type extractor: use the Error helper in the Cause.Cause namespace to extract the error type.

- `Micro.MicroCause.Fail` -> `Cause.Fail`: The Fail variant is a Reason in v4: Cause.Fail\<E\> from effect/Cause.

- `Micro.MicroCause.Interrupt` -> `Cause.Interrupt`: The Interrupt variant is a Reason in v4: Cause.Interrupt from effect/Cause.

- `Micro.MicroCause.Proto` -> `Cause.Cause.ReasonProto`: Internal prototype type; the closest v4 equivalent is the ReasonProto interface in the Cause.Cause namespace. Rarely needed directly.

- `Micro.MicroCauseTypeId` -> `Cause.TypeId`: Use Cause.TypeId from effect/Cause (value is "\~effect/Cause").

- `Micro.MicroExit` -> `Exit.Exit`: MicroExit\<A, E\> becomes Exit.Exit\<A, E\> from effect/Exit. In v4 Exit is a subtype of Effect.

- `Micro.MicroExit.Failure` -> `Exit.Failure`: MicroExit.Failure becomes Exit.Failure\<A, E\> from effect/Exit.

- `Micro.MicroExit.Proto` -> `Exit.Exit.Proto`: Internal prototype type; v4 exposes the shared base as Proto in the Exit.Exit namespace. Rarely needed directly.

- `Micro.MicroExit.Success` -> `Exit.Success`: MicroExit.Success becomes Exit.Success\<A, E\> from effect/Exit.

- `Micro.MicroExitTypeId` -> `none`: v4 Exit is a subtype of Effect and has no dedicated TypeId; use Exit.isExit to identify exits.

- `Micro.MicroFiber` -> `Fiber.Fiber`: MicroFiber\<A, E\> becomes Fiber.Fiber\<A, E\> from effect/Fiber.

- `Micro.MicroFiber.Variance` -> `none`: Type-level variance helper with no public v4 equivalent; the v4 Fiber.Fiber interface carries variance directly.

- `Micro.MicroFiberTypeId` -> `none`: No public TypeId on v4 fibers; use Fiber.isFiber to identify fibers.

- `Micro.MicroIterator` -> `Effect.EffectIterator`: Renamed: MicroIterator becomes Effect.EffectIterator (generator support for Effect.gen).

- `Micro.MicroSchedule` -> `Schedule.Schedule`: v3 MicroSchedule was a plain function (attempt, elapsedMillis) =\> Option\<delayMillis\>; v4 uses the first-class Schedule.Schedule type from effect/Schedule.

- `Micro.MicroScheduler` -> `Scheduler.Scheduler`: The scheduler interface lives in effect/Scheduler in v4.

- `Micro.MicroSchedulerDefault` -> `Scheduler.MixedScheduler`: The default task scheduler implementation in v4 is Scheduler.MixedScheduler from effect/Scheduler.

- `Micro.MicroScope` -> `Scope.Scope`: MicroScope becomes Scope.Scope from effect/Scope; the closeable variant is Scope.Closeable.

- `Micro.MicroScopeTypeId` -> `none`: No public TypeId on v4 scopes; use the Scope.Scope service key to access the current scope.

- `Micro.MicroTypeLambda` -> `Effect.EffectTypeLambda`: Renamed: MicroTypeLambda becomes Effect.EffectTypeLambda.

- `Micro.MicroUnify` -> `Effect.EffectUnify`: Renamed: MicroUnify becomes Effect.EffectUnify.

- `Micro.MicroUnifyIgnore` -> `none`: Removed; v4 Effect declares its unify-ignore slot inline and exposes no named UnifyIgnore interface.

- `Micro.NoSuchElementException` -> `Cause.NoSuchElementError`: Renamed and moved: NoSuchElementException becomes Cause.NoSuchElementError from effect/Cause.

- `Micro.TaggedError` -> `Data.TaggedError`: The yieldable tagged error class constructor is Data.TaggedError from effect/Data in v4.

- `Micro.TimeoutException` -> `Cause.TimeoutError`: Renamed and moved: TimeoutException becomes Cause.TimeoutError from effect/Cause (raised by Effect.timeout).

- `Micro.TypeId` -> `Effect.TypeId`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.YieldableError` -> `Cause.YieldableError`: Moved: YieldableError lives in effect/Cause in v4.

- `Micro.acquireUseRelease` -> `Effect.acquireUseRelease`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.all` -> `Effect.all`: Micro was removed in v4; use Effect.all with the same iterable-or-record input and concurrency/discard options.

- `Micro.as` -> `Effect.as`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.asSome` -> `Effect.asSome`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.asVoid` -> `Effect.asVoid`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

#### `Micro.async`

**Replacement:** `Effect.callback`

Renamed: the async constructor is Effect.callback in v4. Same resume/AbortSignal semantics.

**Example**

```ts
Effect.callback<number>((resume) => resume(Effect.succeed(1)))
```

- `Micro.bind` -> `Effect.bind`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.bindTo` -> `Effect.bindTo`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.catchAll` -> `Effect.catch`: Renamed: catchAll is Effect.catch in v4.

- `Micro.catchAllCause` -> `Effect.catchCause`: Renamed: catchAllCause is Effect.catchCause in v4.

- `Micro.catchAllDefect` -> `Effect.catchDefect`: Renamed: catchAllDefect is Effect.catchDefect in v4.

- `Micro.catchIf` -> `Effect.catchIf`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.catchTag` -> `Effect.catchTag`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.causeDie` -> `Cause.die`: MicroCause was replaced by the unified effect/Cause module in v4.

- `Micro.causeFail` -> `Cause.fail`: MicroCause was replaced by the unified effect/Cause module in v4.

- `Micro.causeInterrupt` -> `Cause.interrupt`: MicroCause was replaced by the unified effect/Cause module in v4. Takes an optional fiber id.

- `Micro.causeIsDie` -> `Cause.hasDies`: v4 Cause aggregates multiple reasons, so tag refinements become reason queries: Cause.hasDies checks for Die reasons; use Cause.isDieReason for a single Reason value.

- `Micro.causeIsFail` -> `Cause.hasFails`: v4 Cause aggregates multiple reasons, so tag refinements become reason queries: Cause.hasFails checks for Fail reasons; use Cause.isFailReason for a single Reason value.

- `Micro.causeIsInterrupt` -> `Cause.hasInterrupts`: v4 Cause aggregates multiple reasons, so tag refinements become reason queries: Cause.hasInterrupts checks for Interrupt reasons (see also Cause.hasInterruptsOnly).

- `Micro.causeSquash` -> `Cause.squash`: Same behavior in the unified effect/Cause module.

- `Micro.causeWithTrace` -> `Cause.annotate`: v4 causes carry structured annotations instead of a traces array; attach trace data with Cause.annotate (e.g. the Cause.StackTrace service). v4 also captures failure stack traces automatically.

- `Micro.context` -> `Effect.context`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.delay` -> `Effect.delay`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.either` -> `Effect.result`: Either was replaced by Result in v4: Effect.result yields Result.Result\<A, E\> instead of Either\<A, E\>.

- `Micro.ensuring` -> `Effect.ensuring`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.exit` -> `Effect.exit`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.exitDie` -> `Exit.die`: MicroExit was replaced by the unified effect/Exit module in v4.

- `Micro.exitFail` -> `Exit.fail`: MicroExit was replaced by the unified effect/Exit module in v4.

- `Micro.exitFailCause` -> `Exit.failCause`: MicroExit was replaced by the unified effect/Exit module in v4.

- `Micro.exitInterrupt` -> `Exit.interrupt`: MicroExit was replaced by the unified effect/Exit module in v4. Takes an optional fiber id.

- `Micro.exitIsDie` -> `Exit.hasDies`: v4 exits carry a multi-reason Cause, so tag refinements become reason queries: Exit.hasDies checks the failure cause for Die reasons.

- `Micro.exitIsFail` -> `Exit.hasFails`: v4 exits carry a multi-reason Cause, so tag refinements become reason queries: Exit.hasFails checks the failure cause for Fail reasons.

- `Micro.exitIsFailure` -> `Exit.isFailure`: Same refinement in the unified effect/Exit module.

- `Micro.exitIsInterrupt` -> `Exit.hasInterrupts`: v4 exits carry a multi-reason Cause, so tag refinements become reason queries: Exit.hasInterrupts checks the failure cause for Interrupt reasons.

- `Micro.exitIsSuccess` -> `Exit.isSuccess`: Same refinement in the unified effect/Exit module.

- `Micro.exitSucceed` -> `Exit.succeed`: MicroExit was replaced by the unified effect/Exit module in v4.

- `Micro.exitVoid` -> `Exit.void`: MicroExit was replaced by the unified effect/Exit module in v4.

- `Micro.exitVoidAll` -> `Exit.asVoidAll`: Renamed: exitVoidAll becomes Exit.asVoidAll in the unified effect/Exit module.

- `Micro.fail` -> `Effect.fail`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.failCause` -> `Effect.failCause`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.failCauseSync` -> `Effect.failCauseSync`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.failSync` -> `Effect.failSync`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.fiberAwait` -> `Fiber.await`: Fiber operations moved to the effect/Fiber module in v4.

- `Micro.fiberInterrupt` -> `Fiber.interrupt`: Fiber operations moved to the effect/Fiber module in v4.

- `Micro.fiberInterruptAll` -> `Fiber.interruptAll`: Fiber operations moved to the effect/Fiber module in v4.

- `Micro.fiberJoin` -> `Fiber.join`: Fiber operations moved to the effect/Fiber module in v4.

- `Micro.filter` -> `Effect.filter`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.filterMap` -> `Effect.filterMap`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.filterOrFail` -> `Effect.filterOrFail`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

#### `Micro.filterOrFailCause`

**Replacement:** `Effect.filterOrElse`

No direct equivalent; use Effect.filterOrElse and fail with a cause in the fallback.

**Example**

```ts
Effect.filterOrElse(effect, predicate, { orElse: () => Effect.failCause(Cause.die("invalid")) })
```

- `Micro.flatten` -> `Effect.flatten`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.flip` -> `Effect.flip`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.forkDaemon` -> `Effect.forkDetach`: Renamed: forkDaemon becomes Effect.forkDetach (fork detached from the parent's lifetime).

- `Micro.forkIn` -> `Effect.forkIn`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.forkScoped` -> `Effect.forkScoped`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.fromEither` -> `Effect.fromResult`: Either was replaced by Result in v4: convert Result.Result values with Effect.fromResult.

- `Micro.fromOption` -> `Effect.fromOption`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.ignore` -> `Effect.ignore`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

#### `Micro.ignoreLogged`

**Replacement:** `Effect.ignore`

Removed; log explicitly before ignoring.

**Example**

```ts
effect.pipe(Effect.tapCause((cause) => Effect.logError(cause)), Effect.ignore)
```

- `Micro.interrupt` -> `Effect.interrupt`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.interruptible` -> `Effect.interruptible`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.isMicro` -> `Effect.isEffect`: Micro values are plain Effects in v4; use Effect.isEffect.

- `Micro.isMicroCause` -> `Cause.isCause`: MicroCause was replaced by the unified effect/Cause module in v4.

- `Micro.isMicroExit` -> `Exit.isExit`: MicroExit was replaced by the unified effect/Exit module in v4.

- `Micro.let` -> `Effect.let`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.map` -> `Effect.map`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

#### `Micro.mapErrorCause`

**Replacement:** `Effect.catchCause`

No direct equivalent; transform the cause by catching it and re-failing.

**Example**

```ts
Effect.catchCause(effect, (cause) => Effect.failCause(Cause.map(cause, transformError)))
```

- `Micro.match` -> `Effect.match`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.matchCause` -> `Effect.matchCause`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.matchCauseEffect` -> `Effect.matchCauseEffect`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.onExit` -> `Effect.onExit`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.onInterrupt` -> `Effect.onInterrupt`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.option` -> `Effect.option`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.orDie` -> `Effect.orDie`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.orElseSucceed` -> `Effect.orElseSucceed`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.promise` -> `Effect.promise`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.race` -> `Effect.race`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.raceAll` -> `Effect.raceAll`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.raceFirst` -> `Effect.raceFirst`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

#### `Micro.repeatExit`

**Replacement:** `Effect.repeat`

Removed; use Effect.repeat with while/until/times/schedule options. To inspect failures while looping, run the body through Effect.exit and repeat on the Exit value.

**Example**

```ts
Effect.repeat(Effect.exit(effect), { while: (exit) => Exit.isFailure(exit), times: 3 })
```

- `Micro.replicate` -> `Effect.replicate`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.replicateEffect` -> `Effect.replicateEffect`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.retry` -> `Effect.retry`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.runFork` -> `Effect.runFork`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.runPromise` -> `Effect.runPromise`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.runPromiseExit` -> `Effect.runPromiseExit`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.runSync` -> `Effect.runSync`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.runSyncExit` -> `Effect.runSyncExit`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.sandbox` -> `Effect.sandbox`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.scheduleAddDelay` -> `Schedule.addDelay`: Moved to effect/Schedule; the callback now receives schedule Metadata and returns a Duration (optionally effectful).

- `Micro.scheduleExponential` -> `Schedule.exponential`: Moved to effect/Schedule; takes Duration input instead of raw millis.

- `Micro.scheduleIntersect` -> `Schedule.max`: Intersection (recur while both recur, waiting for the slower) is Schedule.max([self, that]) in v4.

- `Micro.scheduleRecurs` -> `Schedule.recurs`: Moved to effect/Schedule.

- `Micro.scheduleSpaced` -> `Schedule.spaced`: Moved to effect/Schedule; takes Duration input instead of raw millis.

- `Micro.scheduleUnion` -> `Schedule.min`: Union (recur while either recurs, waiting for the faster) is Schedule.min([self, that]) in v4.

#### `Micro.scheduleWithMaxDelay`

**Replacement:** `Schedule.modifyDelay`

No direct equivalent; clamp the delay with Schedule.modifyDelay.

**Example**

```ts
Schedule.modifyDelay(schedule, ({ delay }) => Duration.min(delay, "10 seconds"))
```

#### `Micro.scheduleWithMaxElapsed`

**Replacement:** `Schedule.upTo`

Renamed: cap total elapsed time with Schedule.upTo({ duration }).

**Example**

```ts
Schedule.upTo(schedule, { duration: "30 seconds" })
```

- `Micro.scopeMake` -> `Scope.make`: Moved to effect/Scope: Scope.make returns Effect\<Scope.Closeable\> and accepts an optional finalizer strategy.

- `Micro.scopeUnsafeMake` -> `Scope.makeUnsafe`: Renamed and moved: scopeUnsafeMake becomes Scope.makeUnsafe from effect/Scope.

- `Micro.scoped` -> `Effect.scoped`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.service` -> `service`: Micro was removed in v4, and services are Effects; yield or compose the service key directly in the rewritten Effect runtime.

- `Micro.succeed` -> `Effect.succeed`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.sync` -> `Effect.sync`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.tap` -> `Effect.tap`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.tapDefect` -> `Effect.tapDefect`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.tapError` -> `Effect.tapError`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.tapErrorCause` -> `Effect.tapCause`: Renamed: tapErrorCause becomes Effect.tapCause.

- `Micro.tapErrorCauseIf` -> `Effect.tapCauseIf`: Renamed: tapErrorCauseIf becomes Effect.tapCauseIf (see also Effect.tapCauseFilter for Filter-based matching).

- `Micro.timeout` -> `Effect.timeout`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.timeoutOption` -> `Effect.timeoutOption`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.try` -> `Effect.try`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.tryPromise` -> `Effect.tryPromise`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.uninterruptibleMask` -> `Effect.uninterruptibleMask`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.updateContext` -> `Effect.updateContext`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.updateService` -> `Effect.updateService`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.void` -> `Effect.void`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.when` -> `Effect.when`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

- `Micro.whileLoop` -> `Effect.whileLoop`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.

#### `Micro.withConcurrency`

**Replacement:** `none`

Removed in v4 along with "inherit" concurrency; pass a { concurrency } option directly to each concurrent operation.

**Example**

```ts
Effect.forEach(items, handle, { concurrency: 10 })
```

- `Micro.withMicroFiber` -> `Effect.withFiber`: Renamed: withMicroFiber becomes Effect.withFiber, giving access to the current fiber.

- `Micro.withTrace` -> `Effect.withSpan`: Removed; v4 captures failure stack traces automatically and cause annotations replace the traces array. For named tracing regions use Effect.withSpan.

- `Micro.yieldFlush` -> `none`: Removed; access the current scheduler via the References.Scheduler service and call its flush() method directly if deterministic draining is needed.

- `Micro.yieldNow` -> `Effect.yieldNow`: Micro was removed in v4; the rewritten Effect runtime is itself lightweight and replaces it. Same-name equivalent on effect/Effect.
