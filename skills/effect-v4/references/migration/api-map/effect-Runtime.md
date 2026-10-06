# v3 → v4 API map: effect/Runtime

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Runtime`

- `Runtime.AsyncFiberException` -> `Cause.AsyncFiberError`: The error thrown when synchronous execution encounters an async boundary was renamed.

- `Runtime.Cancel` -> `ReturnType<typeof Effect.runCallback>`: Use the cancellation function returned by runCallback; the named type was removed.

- `Runtime.FiberFailure` -> `none`: The runner error wrapper was removed; use an Exit-returning runner to retain and inspect a structured Cause.

- `Runtime.FiberFailureCauseId` -> `none`: The FiberFailure wrapper and its cause marker were removed; inspect Cause through Exit instead.

- `Runtime.FiberFailureId` -> `none`: The FiberFailure wrapper and its brand were removed; inspect Cause through Exit instead.

- `Runtime.RunCallbackOptions` -> `Effect.RunOptions & { readonly onExit: (exit: Exit.Exit<unknown, unknown>) => void }`: The callback runner now combines Effect.RunOptions with an onExit callback; no named options type is exported.

- `Runtime.RunForkOptions` -> `Effect.RunOptions`: Use common runner options; express scoped forking with Effect.forkIn or Effect.forkScoped.

- `Runtime.Runtime` -> `Context.Context`: Runtime values were removed; carry a Context and invoke the corresponding Effect.run\*With function.

- `Runtime.Runtime.Context` -> `none`: The Runtime context extractor was removed; carry the service union directly on Context.Context.

- `Runtime.defaultRuntime` -> `Context.empty()`: Runtime values were removed; call Effect.run\* directly or use an empty Context with an Effect.run\*With function.

- `Runtime.defaultRuntimeFlags` -> `none`: Runtime flags were removed; configure scheduler yielding, interruptibility, and runtime metrics independently.

- `Runtime.deleteFiberRef` -> `Context.omit`: FiberRefs became Context.Reference values; omit the Reference override from the Context.

- `Runtime.disableRuntimeFlag` -> `none`: Runtime flags were removed; disable the corresponding scheduler, interruptibility, or metric behavior directly.

- `Runtime.enableRuntimeFlag` -> `none`: Runtime flags were removed; enable the corresponding scheduler, interruptibility, or metric behavior directly.

- `Runtime.isAsyncFiberException` -> `Cause.isAsyncFiberError`: Use the renamed guard from Cause.

- `Runtime.isFiberFailure` -> `none`: FiberFailure no longer exists; use an Exit-returning runner and inspect Exit or Cause.

- `Runtime.make` -> `Context.make`: Runtime values were removed; construct the service Context passed to Effect.run\*With instead.

- `Runtime.makeFiberFailure` -> `Cause.squash`: Use Cause.squash only when a Cause must become the value thrown or rejected by a runner.

- `Runtime.runCallback` -> `Effect.runCallbackWith`: Run with the former Runtime's Context; use Effect.runCallback when no services are required.

- `Runtime.runFork` -> `Effect.runForkWith`: Run with the former Runtime's Context; use Effect.runFork when no services are required.

- `Runtime.runPromise` -> `Effect.runPromiseWith`: Run with the former Runtime's Context; use Effect.runPromise when no services are required.

- `Runtime.runPromiseExit` -> `Effect.runPromiseExitWith`: Run with the former Runtime's Context; use Effect.runPromiseExit when no services are required.

- `Runtime.runSync` -> `Effect.runSyncWith`: Run with the former Runtime's Context; use Effect.runSync when no services are required.

- `Runtime.runSyncExit` -> `Effect.runSyncExitWith`: Run with the former Runtime's Context; use Effect.runSyncExit when no services are required.

- `Runtime.setFiberRef` -> `Context.add`: FiberRefs became Context.Reference values; add the Reference override to the Context.

- `Runtime.updateContext` -> `Context transformation + Effect.run*With`: Runtime values were removed; transform the carried Context directly, then pass the result to the corresponding Effect.run\*With function.

- `Runtime.updateFiberRefs` -> `Context.add`: There is no aggregate FiberRefs update; add targeted Context.Reference overrides to the carried Context explicitly.

- `Runtime.updateRuntimeFlags` -> `none`: Runtime flags and aggregate patches were removed; configure each semantic behavior independently.
