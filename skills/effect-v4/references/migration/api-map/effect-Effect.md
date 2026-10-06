# v3 → v4 API map: effect/Effect

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Effect`

- `Effect.Adapter` -> `none`: The generator adapter type was removed; yield Effect values directly inside `Effect.gen`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.All` -> `Effect.All`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.All.ExtractMode` -> `Effect.All.Return`: The `either` extraction helper was removed; use `mode: "result"` and the v4 return helper. Adapt arguments and imports to the v4 API.

- `Effect.Blocked` -> `none`: The request-runtime blocked model is internal; use public `Request` and `RequestResolver` APIs. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.Effect` -> `Effect.Effect`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.Effect.AsEffect` -> `Effect.Effect`: Use the Effect type directly and extract channels with `Effect.Success`, `Effect.Error`, and `Effect.Services`. Adapt arguments and imports to the v4 API.

- `Effect.Effect.Context` -> `Effect.Services`: Use the renamed type-level extractor for required services. Adapt arguments and imports to the v4 API.

- `Effect.Effect.VarianceStruct` -> `Effect.Variance`: Use the v4 variance interface. Adapt arguments and imports to the v4 API.

- `Effect.EffectGenerator` -> `Effect.EffectIterator`: Use the v4 iterator type used by generator delegation. Adapt arguments and imports to the v4 API.

- `Effect.EffectTypeId` -> `Effect.TypeId`: Use the v4 type-level Effect identifier. Adapt arguments and imports to the v4 API.

- `Effect.EffectUnify` -> `Effect.EffectUnify`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.EffectUnifyIgnore` -> `none`: The internal unification-ignore helper is no longer public; rely on v4 Effect inference. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.FunctionWithSpanOptions` -> `Tracer.SpanOptions`: Use the v4 tracing options type when wrapping functions with `Effect.withSpan`. Adapt arguments and imports to the v4 API.

- `Effect.LatchUnify` -> `Latch.Latch`: Latch moved to the standalone `effect/Latch` module; rely on normal v4 inference. Adapt arguments and imports to the v4 API.

- `Effect.LatchUnifyIgnore` -> `none`: The internal Latch unification helper was removed; use `Latch.Latch` directly. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.Permit` -> `Semaphore.Semaphore`: Use the standalone Semaphore API and its `withPermit` / `withPermits` methods. Adapt arguments and imports to the v4 API.

- `Effect.Repeat` -> `Effect.Repeat`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.Repeat.Options` -> `Effect.Repeat.Options`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.Retry` -> `Effect.Retry`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.Retry.Options` -> `Effect.Retry.Options`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.Service` -> `Context.Service`: Replace the `effect` constructor option with `make`. V4 does not generate a `Default` layer or wire `dependencies`; define a `Layer.effect` and provide its dependencies explicitly.

- `Effect.Service.AllowedType` -> `Context.Service`: Service type machinery moved to `Context.Service`; do not reference its internal helper types. Adapt arguments and imports to the v4 API.

- `Effect.Service.Class` -> `Context.Service`: Service classes are now defined with `Context.Service`. Adapt arguments and imports to the v4 API.

- `Effect.Service.HasArguments` -> `Context.Service`: Service constructor typing is handled by `Context.Service`. Adapt arguments and imports to the v4 API.

- `Effect.Service.MakeAccessors` -> `Context.Service`: Use the generated `.use` helper instead of v3 accessor type machinery. Adapt arguments and imports to the v4 API.

- `Effect.Service.MakeArguments` -> `Context.Service`: Pass a `make` Effect in the v4 `Context.Service` options. Adapt arguments and imports to the v4 API.

- `Effect.Service.MakeContext` -> `Context.Service`: Service context typing is inferred by `Context.Service`. Adapt arguments and imports to the v4 API.

- `Effect.Service.MakeDeps` -> `Layer.provide`: Compose service dependencies explicitly with Layers. Adapt arguments and imports to the v4 API.

- `Effect.Service.MakeDepsE` -> `Layer.Error`: Use Layer channel extractors rather than Service internals. Adapt arguments and imports to the v4 API.

- `Effect.Service.MakeDepsIn` -> `Layer.Services`: Use Layer channel extractors rather than Service internals. Adapt arguments and imports to the v4 API.

- `Effect.Service.MakeDepsOut` -> `Layer.Success`: Use Layer channel extractors rather than Service internals. Adapt arguments and imports to the v4 API.

- `Effect.Service.MakeError` -> `Layer.Error`: Use the Layer error-channel extractor. Adapt arguments and imports to the v4 API.

- `Effect.Service.MakeService` -> `Context.Service`: The service shape is inferred by `Context.Service`. Adapt arguments and imports to the v4 API.

- `Effect.Service.ProhibitedType` -> `Context.Service`: Do not reference the removed internal validation type. Adapt arguments and imports to the v4 API.

- `Effect.Tag` -> `Context.Service`: Define services with `Context.Service`; use the generated `.use` helper for accessors. Adapt arguments and imports to the v4 API.

- `Effect.Tag.AllowedType` -> `Context.Service`: Tag validation internals were removed; use `Context.Service` directly. Adapt arguments and imports to the v4 API.

- `Effect.Tag.ProhibitedType` -> `Context.Service`: Tag validation internals were removed; use `Context.Service` directly. Adapt arguments and imports to the v4 API.

- `Effect.Tag.Proxy` -> `Context.Service.use`: Replace proxy accessors with the generated `.use` helper. Adapt arguments and imports to the v4 API.

- `Effect.acquireReleaseInterruptible` -> `Effect.acquireRelease`: Pass `{ interruptible: true }` in the options object. Adapt arguments and imports to the v4 API.

- `Effect.allSuccesses` -> `Effect.all`: Run with `{ mode: "result" }`, then retain `Result.Success` values. Adapt arguments and imports to the v4 API.

- `Effect.allWith` -> `Effect.all`: Wrap `Effect.all(values, options)` in a lambda when a data-last combinator is needed. Adapt arguments and imports to the v4 API.

- `Effect.allowInterrupt` -> `Effect.yieldNow`: Yield to the scheduler to create an interruptible checkpoint. Adapt arguments and imports to the v4 API.

- `Effect.annotateLogs` -> `Effect.annotateLogs`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.ap` -> `Effect.zipWith`: Zip the function effect and value effect, then apply the function in the combiner. Adapt arguments and imports to the v4 API.

- `Effect.asSomeError` -> `Effect.mapError`: Map errors with `Option.some`. Adapt arguments and imports to the v4 API.

- `Effect.async` -> `Effect.callback`: Use the renamed callback constructor. Adapt arguments and imports to the v4 API.

- `Effect.asyncEffect` -> `Effect.callback`: The callback registration may return an Effect cleanup action in v4. Adapt arguments and imports to the v4 API.

- `Effect.bindAll` -> `Effect.bind + Effect.all`: Bind the result of `Effect.all` explicitly in the do-notation pipeline. Adapt arguments and imports to the v4 API.

- `Effect.blocked` -> `none`: The request-runtime blocked constructor is internal; express work with `Effect.request` and a `RequestResolver`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.cacheRequestResult` -> `none`: Direct request-cache mutation was removed; configure request resolution through `RequestResolver`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.cachedFunction` -> `none`: The function memoizer was removed; use `Cache` for keyed effectful caching. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.catch` -> `Effect.catch`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.catchAll` -> `Effect.catch`: Use the shortened v4 error-handler name. Adapt arguments and imports to the v4 API.

- `Effect.catchAllCause` -> `Effect.catchCause`: Use the shortened v4 cause-handler name. Adapt arguments and imports to the v4 API.

- `Effect.catchAllDefect` -> `Effect.catchDefect`: Use the shortened v4 defect-handler name. Adapt arguments and imports to the v4 API.

- `Effect.catchSome` -> `Effect.catchFilter`: Replace the Option-returning partial function with a `Filter` and handler. Adapt arguments and imports to the v4 API.

- `Effect.catchSomeCause` -> `Effect.catchCauseFilter`: Replace the Option-returning partial function with a cause `Filter` and handler. Adapt arguments and imports to the v4 API.

- `Effect.catchSomeDefect` -> `none`: Use `Effect.catchDefect` and branch explicitly, re-dying for unmatched defects. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.cause` -> `Effect.exit`: Inspect `Exit.Failure.cause`; v4 no longer exposes an Effect-only cause extractor. Adapt arguments and imports to the v4 API.

- `Effect.checkInterruptible` -> `none`: Interruptibility introspection was removed; structure the region explicitly with `Effect.interruptible` or `Effect.uninterruptible`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.clock` -> `Clock.Clock`: Services are Effects in v4; yield or compose `Clock.Clock` directly. Adapt imports to the v4 API.

- `Effect.configProviderWith` -> `ConfigProvider.ConfigProvider.use`: Use the ConfigProvider reference's `.use` helper to invoke the effectful callback. Adapt arguments and imports to the v4 API.

- `Effect.console` -> `Console.Console`: Services are Effects in v4; yield or compose `Console.Console` directly. Adapt imports to the v4 API.

- `Effect.consoleWith` -> `Console.Console.use`: Use the Console reference's `.use` helper to invoke the effectful callback. Adapt arguments and imports to the v4 API.

- `Effect.contextWithEffect` -> `Effect.contextWith`: `contextWith` accepts an effectful callback in v4. Adapt arguments and imports to the v4 API.

- `Effect.currentPropagatedSpan` -> `Effect.currentParentSpan`: Use the current parent span representation. Adapt arguments and imports to the v4 API.

- `Effect.custom` -> `none`: The low-level custom instruction constructor was removed; use public constructors such as `Effect.sync`, `Effect.suspend`, or `Effect.callback`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.daemonChildren` -> `Effect.awaitAllChildren`: Use structured child-fiber waiting, or fork explicitly with `Effect.forkDetach` when detachment is intended. Adapt arguments and imports to the v4 API.

- `Effect.descriptor` -> `Effect.fiberId`: The full fiber descriptor was removed; retrieve the current numeric fiber id. Adapt arguments and imports to the v4 API.

- `Effect.descriptorWith` -> `Effect.fiberId + Effect.flatMap`: Read the current fiber id and invoke the callback explicitly. Adapt arguments and imports to the v4 API.

- `Effect.dieMessage` -> `Effect.die`: Construct the desired defect explicitly and pass it to `Effect.die`. Adapt arguments and imports to the v4 API.

- `Effect.dieSync` -> `Effect.suspend + Effect.die`: Evaluate the lazy defect inside `Effect.suspend`. Adapt arguments and imports to the v4 API.

- `Effect.diffFiberRefs` -> `none`: The public FiberRefs diff API was removed; model fiber-local state with context references and scoped `Effect.provideService`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.disconnect` -> `Effect.forkDetach`: Fork explicitly and decide how to await or interrupt the detached Fiber. Adapt arguments and imports to the v4 API.

- `Effect.dropUntil` -> `none`: Use an explicit `Effect.gen` loop for an effectful stopping predicate. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.dropWhile` -> `none`: Use an explicit `Effect.gen` loop, or `Array.dropWhile` when the predicate is pure. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.either` -> `Effect.result`: V4 represents typed success/failure as `Result` instead of `Either`. Adapt arguments and imports to the v4 API.

- `Effect.ensureErrorType` -> `Effect.satisfiesErrorType`: Use the renamed compile-time channel constraint. Adapt arguments and imports to the v4 API.

- `Effect.ensureRequirementsType` -> `Effect.satisfiesServicesType`: Use the renamed compile-time services constraint. Adapt arguments and imports to the v4 API.

- `Effect.ensureSuccessType` -> `Effect.satisfiesSuccessType`: Use the renamed compile-time channel constraint. Adapt arguments and imports to the v4 API.

- `Effect.ensuringChild` -> `Effect.ensuring + Fiber APIs`: Track the child Fiber explicitly and run the finalizer with `Effect.ensuring`. Adapt arguments and imports to the v4 API.

- `Effect.ensuringChildren` -> `Effect.awaitAllChildren + Effect.ensuring`: Use structured child waiting and an explicit finalizer. Adapt arguments and imports to the v4 API.

- `Effect.every` -> `Effect.forEach`: Evaluate predicates with `Effect.forEach`, then test the resulting booleans with `Array.every`. Adapt arguments and imports to the v4 API.

- `Effect.exists` -> `Effect.findFirst`: Find the first value satisfying the effectful predicate and test the returned Option. Adapt arguments and imports to the v4 API.

- `Effect.fiberIdWith` -> `Effect.fiberId + Effect.flatMap`: Read the numeric fiber id and invoke the callback explicitly. Adapt arguments and imports to the v4 API.

- `Effect.filterEffectOrElse` -> `Effect.flatMap`: Evaluate the effectful predicate and branch to `Effect.succeed` or the fallback. Adapt arguments and imports to the v4 API.

- `Effect.filterEffectOrFail` -> `Effect.flatMap`: Evaluate the effectful predicate and branch to `Effect.succeed` or `Effect.fail`. Adapt arguments and imports to the v4 API.

- `Effect.filterOrDie` -> `Effect.filterOrFail + Effect.orDie`: Filter with a typed failure, then convert it to a defect. Adapt arguments and imports to the v4 API.

- `Effect.filterOrDieMessage` -> `Effect.filterOrFail + Effect.orDie`: Create the message-bearing error in `filterOrFail`, then convert it to a defect. Adapt arguments and imports to the v4 API.

- `Effect.finalizersMask` -> `none`: Configurable finalizer execution strategies were removed; register ordered finalizers explicitly in a Scope. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.flipWith` -> `Effect.flip`: Flip, apply the transformation, then flip the resulting Effect back. Adapt arguments and imports to the v4 API.

- `Effect.fn` -> `Effect.fn`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.fn.Gen` -> `Effect.fn.Return`: Use the v4 generator-return helper type. Adapt arguments and imports to the v4 API.

- `Effect.fn.NonGen` -> `Effect.fn.Untraced`: Use the v4 function helper type for non-generator wrapping. Adapt arguments and imports to the v4 API.

- `Effect.fork` -> `Effect.forkChild`: Use the renamed structured child-fiber combinator. Adapt arguments and imports to the v4 API.

- `Effect.forkAll` -> `Effect.forEach + Effect.forkChild`: Fork each effect explicitly, or prefer a higher-level concurrent combinator. Adapt arguments and imports to the v4 API.

- `Effect.forkDaemon` -> `Effect.forkDetach`: Use the renamed detached-fiber combinator. Adapt arguments and imports to the v4 API.

- `Effect.forkWithErrorHandler` -> `Effect.forkChild + Fiber.await`: Fork explicitly and observe the Fiber result to handle errors. Adapt arguments and imports to the v4 API.

- `Effect.fromFiber` -> `Fiber.join`: Join the Fiber to obtain an Effect of its result. Adapt arguments and imports to the v4 API.

- `Effect.fromFiberEffect` -> `Effect.flatMap + Fiber.join`: FlatMap the effectful Fiber and join it. Adapt arguments and imports to the v4 API.

- `Effect.fromNullable` -> `Effect.fromOption + Option.fromNullable`: Convert the nullable value to Option, then lift it into Effect. Adapt arguments and imports to the v4 API.

- `Effect.functionWithSpan` -> `Effect.withSpan`: Wrap the function body with a span whose name/options are derived from its arguments. Adapt arguments and imports to the v4 API.

- `Effect.gen` -> `Effect.gen`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.getFiberRefs` -> `none`: The FiberRefs collection is no longer public; access individual context references through Effect services. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.getRuntimeFlags` -> `none`: RuntimeFlags are no longer a public Effect service; use supported high-level runtime options. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.if` -> `Effect.suspend`: Select the branch lazily with a JavaScript conditional inside `Effect.suspend`. Adapt arguments and imports to the v4 API.

- `Effect.ignoreLogged` -> `Effect.ignore`: Pass `{ log: true }` to the consolidated ignore combinator. Adapt arguments and imports to the v4 API.

- `Effect.inheritFiberRefs` -> `none`: Bulk FiberRef inheritance was removed; propagate required context references explicitly. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.interruptWith` -> `Effect.interrupt`: V4 interruption uses the current fiber identity; remove the explicit FiberId argument. Adapt arguments and imports to the v4 API.

- `Effect.intoDeferred` -> `Deferred.into`: Use the Deferred module combinator. Adapt arguments and imports to the v4 API.

- `Effect.iterate` -> `none`: Use an explicit stateful `Effect.gen` loop; v4 removed the Effect-specific loop helper. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.labelMetrics` -> `Metric.withAttributes`: Convert labels to metric attributes and scope them around the Effect. Adapt arguments and imports to the v4 API.

- `Effect.labelMetricsScoped` -> `Metric.withAttributes`: Apply metric attributes to the scoped Effect rather than mutating scoped labels. Adapt arguments and imports to the v4 API.

- `Effect.let` -> `Effect.let`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.liftPredicate` -> `Effect.filterOrFail`: Lift the input with `Effect.succeed` and apply `filterOrFail`. Adapt arguments and imports to the v4 API.

- `Effect.linkSpanCurrent` -> `Effect.linkSpans`: Use the v4 span-link combinator. Adapt arguments and imports to the v4 API.

- `Effect.locally` -> `Effect.provideService`: FiberRef values are context references in v4; provide the reference for the Effect lifetime. Adapt arguments and imports to the v4 API.

- `Effect.locallyScoped` -> `Effect.provideService`: Provide the context reference around the scoped Effect. Adapt arguments and imports to the v4 API.

- `Effect.locallyScopedWith` -> `Effect.updateServiceScoped`: Context references replace FiberRefs in v4; update the reference for the current scope. Adapt arguments and imports to the v4 API.

- `Effect.locallyWith` -> `Effect.updateService`: Context references replace FiberRefs in v4; update the reference around the target Effect. Adapt arguments and imports to the v4 API.

- `Effect.logAnnotations` -> `References.CurrentLogAnnotations`: Context references are Effects in v4; yield or compose `References.CurrentLogAnnotations` directly. Adapt imports to the v4 API.

- `Effect.loop` -> `none`: Use an explicit `Effect.gen` loop and collect results when needed. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.makeLatch` -> `Latch.make`: Latch constructors moved to `effect/Latch`. Adapt arguments and imports to the v4 API.

- `Effect.makeSemaphore` -> `Semaphore.make`: Semaphore constructors moved to `effect/Semaphore`. Adapt arguments and imports to the v4 API.

- `Effect.mapAccum` -> `Effect.reduce`: Carry `[state, output]` through an effectful reduction. Adapt arguments and imports to the v4 API.

- `Effect.mapErrorCause` -> `Effect.catchCause + Effect.failCause`: Transform the Cause in a cause handler and fail with the mapped Cause. Adapt arguments and imports to the v4 API.

- `Effect.mapInputContext` -> `Effect.contextWith + Effect.provide`: Build the required context from the incoming context and provide it explicitly. Adapt arguments and imports to the v4 API.

- `Effect.merge` -> `Effect.catch`: Recover each typed error with `Effect.succeed` so both channels become success values. Adapt arguments and imports to the v4 API.

- `Effect.mergeAll` -> `Effect.reduce`: Reduce the input effects with an effectful accumulator. Adapt arguments and imports to the v4 API.

- `Effect.metricLabels` -> `Metric.CurrentMetricAttributes`: Context references are Effects in v4; yield or compose `Metric.CurrentMetricAttributes` directly. Adapt imports to the v4 API.

- `Effect.negate` -> `Effect.map`: Map the boolean result with logical negation. Adapt arguments and imports to the v4 API.

- `Effect.none` -> `Effect.flatMap + Option.match`: Fail for `Some` and succeed with void for `None`. Adapt arguments and imports to the v4 API.

- `Effect.once` -> `Effect.cached`: Create the cached Effect once, then execute the returned Effect repeatedly. Adapt arguments and imports to the v4 API.

- `Effect.optionFromOptional` -> `Effect.catchTag`: Map success to `Option.some` and recover `NoSuchElementError` with `Option.none`. Adapt arguments and imports to the v4 API.

- `Effect.orDieWith` -> `Effect.mapError + Effect.orDie`: Map the typed error to the desired defect, then convert failures to defects. Adapt arguments and imports to the v4 API.

- `Effect.orElse` -> `Effect.catch`: Ignore the caught error and evaluate the fallback Effect. Adapt arguments and imports to the v4 API.

- `Effect.orElseFail` -> `Effect.mapError`: Replace every typed error with the lazily produced failure value. Adapt arguments and imports to the v4 API.

- `Effect.parallelErrors` -> `Effect.all`: Use `{ mode: "result", concurrency: "unbounded" }` and collect failures explicitly. Adapt arguments and imports to the v4 API.

- `Effect.parallelFinalizers` -> `none`: Parallel finalizer strategy mutation was removed; fork independent cleanup explicitly when ordering is irrelevant. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.partition` -> `Effect.partition`: The tuple is now [passes, fails], the reverse of v3 [failures, successes]. Every effect still runs, and the outer effect does not fail.

- `Effect.patchFiberRefs` -> `none`: Bulk FiberRefs patching was removed; update individual context references with `Effect.updateService`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.patchRuntimeFlags` -> `none`: RuntimeFlags patching was removed from the public API; use supported high-level runtime options. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.raceWith` -> `Effect.raceFirst + Fiber APIs`: Use `raceFirst` for first completion, or fork both effects and inspect their Exits for custom finishers. Adapt arguments and imports to the v4 API.

- `Effect.random` -> `Random.Random`: Services are Effects in v4; yield or compose `Random.Random` directly. Adapt imports to the v4 API.

- `Effect.randomWith` -> `Random.Random.use`: Use the Random reference's `.use` helper to invoke the effectful callback. Prefer module-level Random operations when possible.

- `Effect.reduceEffect` -> `Effect.flatMap + Effect.reduce`: Evaluate the initial Effect, then reduce the remaining effects. Adapt arguments and imports to the v4 API.

- `Effect.reduceRight` -> `Effect.reduce`: Reverse the input first, then perform the effectful reduction. Adapt arguments and imports to the v4 API.

- `Effect.reduceWhile` -> `none`: Use an explicit `Effect.gen` loop that checks the accumulator before each step. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.repeatN` -> `Effect.repeat`: Pass `{ times: n }` to the consolidated repeat combinator. Adapt arguments and imports to the v4 API.

- `Effect.runRequestBlock` -> `none`: The request-runtime block runner is internal; submit requests with `Effect.request`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.runtime` -> `Effect.context + Effect.runForkWith`: Capture services as a Context and use the corresponding `run*With` function. Adapt arguments and imports to the v4 API.

- `Effect.scheduleForked` -> `Effect.schedule + Effect.forkScoped`: Schedule the Effect, then fork it in the current Scope. Adapt arguments and imports to the v4 API.

- `Effect.scopeWith` -> `Effect.scopedWith`: Use the renamed scoped callback combinator. Adapt arguments and imports to the v4 API.

- `Effect.sequentialFinalizers` -> `none`: Sequential reverse-order finalization is the normal Scope behavior; remove this wrapper. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.serviceConstants` -> `Context.Service.use`: Expose constants from the service explicitly or through the generated `use` helper. Adapt arguments and imports to the v4 API.

- `Effect.serviceFunction` -> `Context.Service.use`: Use the service class `.use` helper to build an accessor function. Adapt arguments and imports to the v4 API.

- `Effect.serviceFunctionEffect` -> `Context.Service.use`: Use the service class `.use` helper for effect-returning methods. Adapt arguments and imports to the v4 API.

- `Effect.serviceFunctions` -> `Context.Service.use`: Define explicit service accessors with the generated `.use` helper. Adapt arguments and imports to the v4 API.

- `Effect.serviceMembers` -> `Context.Service.use`: Define explicit service accessors with the generated `.use` helper. Adapt arguments and imports to the v4 API.

- `Effect.serviceOptional` -> `service`: Services are Effects in v4; yield or compose the service key directly. Use `Effect.serviceOption` only when absence is expected.

- `Effect.setFiberRefs` -> `none`: Bulk FiberRefs replacement was removed; provide individual context references. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.step` -> `none`: The low-level Effect stepping API was removed from the public surface. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.summarized` -> `Effect.gen`: Run the summary Effect before and after the target Effect and combine the two measurements explicitly. Adapt arguments and imports to the v4 API.

- `Effect.supervised` -> `FiberSet`: Track explicitly forked Fibers in a scoped `FiberSet` instead of installing a runtime Supervisor. Adapt arguments and imports to the v4 API.

- `Effect.tagMetrics` -> `Metric.withAttributes`: Convert key/value tags to metric attributes. Adapt arguments and imports to the v4 API.

- `Effect.tagMetricsScoped` -> `Metric.withAttributes`: Apply attributes around the scoped Effect. Adapt arguments and imports to the v4 API.

- `Effect.takeUntil` -> `none`: Use an explicit `Effect.gen` loop for an effectful stopping predicate. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.takeWhile` -> `none`: Use an explicit `Effect.gen` loop, or `Array.takeWhile` when the predicate is pure. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.tapBoth` -> `Effect.tapError + Effect.tap`: Tap the failure path first, then tap successful values. Adapt arguments and imports to the v4 API.

- `Effect.tapErrorCause` -> `Effect.tapCause`: Use the shortened v4 cause-tap name. Adapt arguments and imports to the v4 API.

- `Effect.timedWith` -> `Effect.gen`: Read the supplied clock Effect before and after the target and compute the Duration explicitly. Adapt arguments and imports to the v4 API.

- `Effect.timeoutFail` -> `Effect.timeoutOrElse`: Use `Effect.fail(onTimeout())` as the timeout fallback. Adapt arguments and imports to the v4 API.

- `Effect.timeoutFailCause` -> `Effect.timeoutOrElse`: Use `Effect.failCause(onTimeout())` as the timeout fallback. Adapt arguments and imports to the v4 API.

- `Effect.timeoutTo` -> `Effect.timeoutOrElse + Effect.map`: Map successful values first and use the timeout fallback for `onTimeout`. Adapt arguments and imports to the v4 API.

- `Effect.tracerWith` -> `Tracer.Tracer.use`: Use the Tracer reference's `.use` helper to invoke the effectful callback. Adapt arguments and imports to the v4 API.

- `Effect.transplant` -> `none`: Fiber scope grafting was removed; use structured concurrency with `forkChild`, `forkScoped`, or `forkIn`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.transposeMapOption` -> `Option.match`: Return `Effect.succeedNone` for None and map the Effect result to Some. Adapt arguments and imports to the v4 API.

- `Effect.try` -> `Effect.try`: Use the callback overload for Cause.UnknownError, or the object overload with try and catch to map failures to a custom error. The callback-only overload does not accept a custom error type parameter.

- `Effect.tryMap` -> `Effect.flatMap + Effect.try`: FlatMap the source value into the v4 synchronous try constructor. Adapt arguments and imports to the v4 API.

- `Effect.tryMapPromise` -> `Effect.flatMap + Effect.tryPromise`: FlatMap the source value into the v4 Promise try constructor. Adapt arguments and imports to the v4 API.

- `Effect.unless` -> `Effect.suspend`: Select `Effect.void` or the target Effect with a negated lazy condition. Adapt arguments and imports to the v4 API.

- `Effect.unlessEffect` -> `Effect.when`: Negate the effectful boolean condition, then use the consolidated `when`. Adapt arguments and imports to the v4 API.

- `Effect.unsafeMakeLatch` -> `Latch.makeUnsafe`: The unsafe constructor moved to `effect/Latch`. Adapt arguments and imports to the v4 API.

- `Effect.unsafeMakeSemaphore` -> `Semaphore.makeUnsafe`: The unsafe constructor moved to `effect/Semaphore`. Adapt arguments and imports to the v4 API.

- `Effect.unsandbox` -> `Effect.catch + Effect.failCause`: Treat the sandboxed Cause as an error and fail with that Cause. Adapt arguments and imports to the v4 API.

- `Effect.updateFiberRefs` -> `none`: Bulk FiberRefs updates were removed; update individual context references with `Effect.updateService`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.using` -> `Effect.scoped + Effect.flatMap`: Acquire inside a fresh Scope, run the use Effect, and close the Scope afterward. Adapt arguments and imports to the v4 API.

- `Effect.validateAll` -> `Effect.validate`: Use the consolidated collection validation combinator. Adapt arguments and imports to the v4 API.

- `Effect.validateFirst` -> `Effect.firstSuccessOf`: Map inputs to validation effects and select the first success; handle accumulated diagnostics explicitly if required. Adapt arguments and imports to the v4 API.

- `Effect.validateWith` -> `Effect.zipWith`: Zip and combine the Effects; use `mode: "result"` when both failures must be retained. Adapt arguments and imports to the v4 API.

- `Effect.whenEffect` -> `Effect.when`: The v4 `when` combinator accepts an effectful boolean condition directly. Adapt arguments and imports to the v4 API.

- `Effect.whenFiberRef` -> `reference.use + Effect.when`: Use the Context.Reference `.use` helper to inspect the value, test it, and branch explicitly. Adapt arguments and imports to the v4 API.

- `Effect.whenLogLevel` -> `none`: Log-level conditional execution was removed; configure Logger filtering and guard optional work explicitly. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.whenRef` -> `Ref.get + Effect.flatMap`: Read the Ref, test it, and branch explicitly. Adapt arguments and imports to the v4 API.

- `Effect.withClock` -> `Effect.provideService`: Provide `Clock.Clock` for the target Effect. Adapt arguments and imports to the v4 API.

- `Effect.withClockScoped` -> `Effect.provideService`: Provide `Clock.Clock` around the scoped Effect. Adapt arguments and imports to the v4 API.

- `Effect.withConcurrency` -> `none`: Ambient concurrency was removed; pass `concurrency` directly to `Effect.all`, `Effect.forEach`, and related combinators. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.withConfigProvider` -> `Effect.provideService`: Provide `ConfigProvider.ConfigProvider` for the target Effect. Adapt arguments and imports to the v4 API.

- `Effect.withConfigProviderScoped` -> `Effect.provideService`: Provide the ConfigProvider around the scoped Effect. Adapt arguments and imports to the v4 API.

- `Effect.withConsole` -> `Effect.provideService`: Provide `Console.Console` for the target Effect. Adapt arguments and imports to the v4 API.

- `Effect.withConsoleScoped` -> `Effect.provideService`: Provide the Console service around the scoped Effect. Adapt arguments and imports to the v4 API.

- `Effect.withEarlyRelease` -> `Scope.make + Scope.close`: Create a Scope explicitly, provide it to acquisition, and retain a close action. Adapt arguments and imports to the v4 API.

- `Effect.withFiberRuntime` -> `none`: Direct FiberRuntime access was removed; use public Effect, Fiber, and Context operations. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.withLogSpan` -> `Effect.withLogSpan`: Still exported in v4; update call sites for the revised signature, options, and channel inference.

- `Effect.withMaxOpsBeforeYield` -> `none`: The scheduler operation budget is no longer configurable through Effect. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.withMetric` -> `Effect.tap + Metric.update`: Update the Metric explicitly from the Effect success value. Adapt arguments and imports to the v4 API.

- `Effect.withRandom` -> `Effect.provideService`: Provide `Random.Random` for the target Effect. Adapt arguments and imports to the v4 API.

- `Effect.withRandomFixed` -> `Effect.provideService`: Provide a custom deterministic `Random.Random` implementation. Adapt arguments and imports to the v4 API.

- `Effect.withRandomScoped` -> `Effect.provideService`: Provide the Random service around the scoped Effect. Adapt arguments and imports to the v4 API.

- `Effect.withRequestBatching` -> `none`: Ambient request batching configuration was removed; configure batching in the `RequestResolver`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.withRequestCache` -> `none`: Ambient request-cache replacement was removed; model keyed caching explicitly with `Cache`. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.withRequestCaching` -> `none`: Ambient request caching was removed; configure resolution or use `Cache` explicitly. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.withRuntimeFlagsPatch` -> `none`: RuntimeFlags patching was removed from the public API. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.withRuntimeFlagsPatchScoped` -> `none`: Scoped RuntimeFlags patching was removed from the public API. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.withScheduler` -> `none`: Ambient scheduler replacement was removed; use supported runtime run options or explicit scheduling combinators. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.withSchedulingPriority` -> `none`: Ambient fiber scheduling priority was removed from the public API. No direct public replacement exists in v4; rewrite the call site around the stated v4 primitive.

- `Effect.withTracerScoped` -> `Effect.provideService`: Provide the Tracer service around the scoped Effect. Adapt arguments and imports to the v4 API.

- `Effect.withUnhandledErrorLogLevel` -> `Effect.ignore`: Handle or explicitly ignore child-fiber failures, selecting the desired log behavior at the boundary. Adapt arguments and imports to the v4 API.

- `Effect.zipLeft` -> `Effect.zip + Effect.map`: Zip the Effects and select the first tuple element. Adapt arguments and imports to the v4 API.

- `Effect.zipRight` -> `Effect.andThen`: Sequence the Effects and retain the second result. Adapt arguments and imports to the v4 API.
