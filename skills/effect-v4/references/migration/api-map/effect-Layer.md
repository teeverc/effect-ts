# v3 → v4 API map: effect/Layer

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Layer`

- `Layer.CurrentMemoMap` -> `Layer.CurrentMemoMap`: The service remains but is now a Context.Service class with forkOrCreate.

- `Layer.Layer` -> `Layer.Layer`: The type remains with Layer\<ROut, E, RIn\> parameter order.

- `Layer.Layer.Context` -> `Layer.Services<T>`: The input-services extractor moved to the module level and was renamed.

- `Layer.LayerTypeId` -> `none`: The marker is private in v4; use Layer.Any or Layer.Variance for type constraints.

- `Layer.MemoMap` -> `Layer.MemoMap`: The interface remains and now supports parent-child ambient maps.

- `Layer.MemoMapTypeId` -> `none`: The MemoMap marker is private in v4.

- `Layer.annotateLogs` -> `Layer.fromBuild((memoMap, scope) => Effect.annotateLogs(Layer.buildWithMemoMap(self, memoMap, scope), ...annotations))`: Apply Effect.annotateLogs to the layer acquisition effect.

- `Layer.annotateSpans` -> `Layer.fromBuild((memoMap, scope) => Effect.annotateSpans(Layer.buildWithMemoMap(self, memoMap, scope), ...annotations))`: Apply Effect.annotateSpans to the layer acquisition effect.

- `Layer.catchAll` -> `Layer.catch`: The typed-error handler was renamed.

- `Layer.catchAllCause` -> `Layer.catchCause`: The cause handler was renamed.

- `Layer.context` -> `Layer.effectContext(Effect.context<R>())`: Capture and return the current service context.

- `Layer.die` -> `Layer.unwrap(Effect.die(defect))`: Lift the Effect defect constructor.

- `Layer.dieSync` -> `Layer.unwrap(Effect.suspend(() => Effect.die(evaluate())))`: Suspend evaluation and lift Effect.die; Effect.dieSync was also removed.

- `Layer.discard` -> `Layer.flatMap(self, () => Layer.empty)`: Build the layer while dropping its output context.

- `Layer.ensureErrorType` -> `Layer.satisfiesErrorType`: The type constraint helper was renamed.

- `Layer.ensureRequirementsType` -> `Layer.satisfiesServicesType`: The requirements type constraint was renamed to services.

- `Layer.ensureSuccessType` -> `Layer.satisfiesSuccessType`: The type constraint helper was renamed.

- `Layer.extendScope` -> `Layer.buildWithScope(self, outerScope) and Effect.provideContext(program, context)`: Explicitly build against the desired outer scope and provide the resulting context.

- `Layer.fail` -> `Layer.unwrap(Effect.fail(error))`: Lift the Effect failure constructor.

- `Layer.failCause` -> `Layer.unwrap(Effect.failCause(cause))`: Lift the Effect cause-failure constructor.

- `Layer.failCauseSync` -> `Layer.unwrap(Effect.failCauseSync(evaluate))`: Lift the retained Effect constructor.

- `Layer.failSync` -> `Layer.unwrap(Effect.failSync(evaluate))`: Lift the retained Effect constructor.

- `Layer.fiberRefLocallyScopedWith` -> `Layer.effect(reference, Effect.map(reference, f))`: FiberRef was removed; compute and provide a transformed Context.Reference value.

- `Layer.flatten` -> `Layer.flatMap(self, (context) => Context.get(context, key))`: Expand the removed convenience combinator with flatMap and Context.get.

- `Layer.function` -> `Layer.effect(keyB, Effect.map(keyA, f))`: Read the input service through its Context.Key and provide the transformed service.

- `Layer.isFresh` -> `none`: Layer.fresh remains, but its wrapper has no public freshness predicate.

- `Layer.locally` -> `Layer.updateService(self, reference, () => value)`: Replace FiberRef-local configuration with Context.Reference provision.

- `Layer.locallyEffect` -> `Layer.fromBuild((memoMap, scope) => f(Layer.buildWithMemoMap(self, memoMap, scope)))`: Transform the public layer acquisition effect directly.

- `Layer.locallyScoped` -> `Layer.succeed(reference, value)`: Provide a v4 Context.Reference value as a configuration layer.

- `Layer.locallyWith` -> `Layer.updateService(self, reference, f)`: Transform a Context.Reference during layer acquisition.

- `Layer.map` -> `Layer.flatMap(self, (context) => Layer.succeedContext(f(context)))`: Expand the removed output-context mapping combinator.

- `Layer.mapError` -> `Layer.fromBuild((memoMap, scope) => Effect.mapError(Layer.buildWithMemoMap(self, memoMap, scope), f))`: Transform the typed error of layer acquisition.

- `Layer.match` -> `Layer.fromBuild with Effect.matchEffect over Layer.buildWithMemoMap`: Fold the source acquisition effect, then build the selected failure or success layer.

- `Layer.matchCause` -> `Layer.fromBuild with Effect.matchCauseEffect over Layer.buildWithMemoMap`: Fold the source acquisition cause, then build the selected failure or success layer.

- `Layer.memoize` -> `automatic shared memoization under Effect.provide`: Reuse the same Layer value; use { local: true } or Layer.fresh to opt out, or MemoMap APIs for manual control.

- `Layer.orElse` -> `Layer.catch(self, () => fallback())`: Expand the removed lazy fallback alias with Layer.catch.

- `Layer.passthrough` -> `Layer.merge(Layer.effectContext(Effect.context<RIn>()), self)`: Capture required input services and merge them into the layer output.

- `Layer.project` -> `Layer.flatMap(self, (context) => Layer.succeed(keyB, f(Context.get(context, keyA))))`: Project one derived service and drop the other outputs.

- `Layer.retry` -> `Effect.retry(acquire, schedule) before Layer.effect or Layer.effectContext`: Retry the acquisition Effect; for an arbitrary layer, rebuild a fresh layer for each attempt through Layer.fromBuild.

- `Layer.scope` -> `Layer.effect(Scope.Scope, Effect.acquireRelease(Scope.make(), Scope.close))`: Construct and close a child scope explicitly.

- `Layer.scoped` -> `Layer.effect`: Scoped acquisition was merged into Layer.effect, which supplies and excludes the layer Scope.

- `Layer.scopedContext` -> `Layer.effectContext`: Scoped context acquisition was merged into Layer.effectContext.

- `Layer.scopedDiscard` -> `Layer.effectDiscard`: Scoped discard acquisition was merged into Layer.effectDiscard.

- `Layer.service` -> `Layer.effect(key, key)`: A Context.Key is an Effect that reads and passes through its service.

- `Layer.setClock` -> `Layer.succeed(Clock.Clock, clock)`: Clock.Clock is now a Context.Reference; provide it directly.

- `Layer.setConfigProvider` -> `ConfigProvider.layer(configProvider)`: Use the dedicated ConfigProvider layer constructor.

- `Layer.setRandom` -> `Layer.succeed(Random.Random, random)`: Random.Random is now a Context.Reference; provide it directly.

- `Layer.setRequestBatching` -> `none`: Requests now use resolver-driven batching and expose no batching switch.

- `Layer.setRequestCache` -> `none`: The public Request.Cache and its configuration API were removed.

- `Layer.setRequestCaching` -> `none`: The public request-caching toggle was removed.

- `Layer.setScheduler` -> `Layer.succeed(Scheduler.Scheduler, scheduler)`: Scheduler.Scheduler is now a Context.Reference; provide it directly.

- `Layer.setTracer` -> `Layer.succeed(Tracer.Tracer, tracer)`: Tracer.Tracer is now a Context.Reference; provide it directly.

- `Layer.setTracerEnabled` -> `Layer.succeed(References.TracerEnabled, enabled)`: Provide the v4 Reference instead of setting a FiberRef.

- `Layer.setTracerTiming` -> `Layer.succeed(References.TracerTimingEnabled, enabled)`: Provide the renamed v4 Reference instead of setting a FiberRef.

- `Layer.setUnhandledErrorLogLevel` -> `Layer.succeed(References.UnhandledLogLevel, severityOrUndefined)`: Provide LogLevel.Severity or undefined instead of Option\<LogLevel\>.

- `Layer.setVersionMismatchErrorLogLevel` -> `none`: No version-mismatch log-level Reference or public replacement exists.

- `Layer.tapErrorCause` -> `Layer.tapCause`: The cause observer was renamed. Its callback must accept the source layer's full error cause; a callback narrowed to only part of the error union is rejected.

- `Layer.toRuntime` -> `Layer.build(self), then Effect.runForkWith, Effect.runPromiseWith, or Effect.runSyncWith`: Runtime\<R\> was removed; build a Context, or use ManagedRuntime.make for a reusable managed runner.

- `Layer.toRuntimeWithMemoMap` -> `Layer.buildWithMemoMap(self, memoMap, scope), then Effect.run*With(context)`: Explicit memo-map building now yields a Context rather than a Runtime.

- `Layer.unwrapEffect` -> `Layer.unwrap`: The Effect-based unwrap constructor was renamed and generalized.

- `Layer.unwrapScoped` -> `Layer.unwrap`: Scoped and unscoped unwrap were merged; Layer.unwrap supplies and excludes the layer Scope.

- `Layer.updateService` -> `Layer.updateService`: The combinator remains and now accepts any Context.Key.

- `Layer.zipWith` -> `Layer.fromBuild with concurrent Effect.zipWith over Layer.buildWithMemoMap`: Combine acquisition effects directly; use Layer.merge when the function only merged Context values.
