# v3 → v4 API map: effect/TestServices

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TestServices`

- `TestServices.TestServices` -> `TestClock.TestClock | TestConsole.TestConsole`: This is the v4 @effect/vitest test-environment union. it.effect provides both automatically; it.live provides neither override.

- `TestServices.annotate` -> `none`: The annotation service was removed. Use Vitest metadata/options for runner concerns, an ordinary Ref or Context.Reference for application-owned test state, and FiberSet for explicit fiber tracking.

- `TestServices.annotations` -> `none`: The annotation service was removed. Use Vitest metadata/options for runner concerns, an ordinary Ref or Context.Reference for application-owned test state, and FiberSet for explicit fiber tracking.

- `TestServices.annotationsLayer` -> `none`: The annotation service was removed. Use Vitest metadata/options for runner concerns, an ordinary Ref or Context.Reference for application-owned test state, and FiberSet for explicit fiber tracking.

- `TestServices.annotationsWith` -> `none`: The annotation service was removed. Use Vitest metadata/options for runner concerns, an ordinary Ref or Context.Reference for application-owned test state, and FiberSet for explicit fiber tracking.

- `TestServices.currentServices` -> `Effect.context()`: The separate FiberRef\<Context\<TestServices\>\> was removed. Test services now live in the ordinary Effect Context; override individual references with Effect.provideService.

- `TestServices.get` -> `none`: The annotation service was removed. Use Vitest metadata/options for runner concerns, an ordinary Ref or Context.Reference for application-owned test state, and FiberSet for explicit fiber tracking.

- `TestServices.liveLayer` -> `none`: The standalone TestLive service and layer were removed. TestClock.layer() captures its surrounding live Clock itself.

- `TestServices.liveServices` -> `none`: There is no prebuilt aggregate test-service Context. @effect/vitest constructs TestClock and TestConsole layers per test; live references are defaults.

- `TestServices.liveWith` -> `TestClock.withLive`: There is no TestLive callback object. Refactor to the effect ultimately run and apply TestClock.withLive, or use it.live for whole-test live execution.

- `TestServices.provideLive` -> `TestClock.withLive`: For live time, run the effect with the Clock captured by TestClock.layer(). Use it.live when the entire test should omit all test-service overrides.

- `TestServices.provideWithLive` -> `TestClock.testClockWith + TestClock.withLive + Effect.provideService`: To retain test time for the inner effect while its transformer uses live time, combine testClockWith, withLive, and provideService. Other v3 default services have no aggregate equivalent.

- `TestServices.repeats` -> `Vitest TestOptions.repeats`: Configure repeats in the Vitest options passed to it.effect or it.live; it is no longer an Effect service value.

- `TestServices.retries` -> `Vitest TestOptions.retry`: Configure retry in Vitest test options. To retry an Effect inside a test, use Effect.retry.

- `TestServices.samples` -> `{ arbitrary: { runs } }`: Pass the run count through @effect/vitest property-test options, for example it.effect.prop(..., { arbitrary: { runs: samples } }).

- `TestServices.shrinks` -> `{ arbitrary: { maxShrinks } }`: Pass maxShrinks through @effect/vitest property-test options, for example it.effect.prop(..., { arbitrary: { maxShrinks } }).

- `TestServices.size` -> `CurrentSize`: Define a custom Context.Reference\<number\> and yield it to read the current size.

- `TestServices.sized` -> `CurrentSize`: TestSized was removed. Use a custom Context.Reference\<number\> directly instead of a wrapper object.

- `TestServices.sizedLayer` -> `Layer.succeed(CurrentSize, size)`: Provide the custom size reference as a layer.

- `TestServices.sizedWith` -> `CurrentSize.use`: Use the custom reference's callback, or preferably yield CurrentSize in Effect.gen.

- `TestServices.supervisedFibers` -> `none`: The annotation service was removed. Use Vitest metadata/options for runner concerns, an ordinary Ref or Context.Reference for application-owned test state, and FiberSet for explicit fiber tracking.

- `TestServices.testConfig` -> `none`: The runner no longer reads an Effect TestConfig service. Use Vitest TestOptions and property-test arbitrary options; model application state as a custom Context.Reference.

- `TestServices.testConfigLayer` -> `none`: The runner no longer reads an Effect TestConfig service. Use Vitest TestOptions and property-test arbitrary options; model application state as a custom Context.Reference.

- `TestServices.testConfigWith` -> `none`: The runner no longer reads an Effect TestConfig service. Use Vitest TestOptions and property-test arbitrary options; model application state as a custom Context.Reference.

- `TestServices.withAnnotations` -> `none`: The annotation service was removed. Use Vitest metadata/options for runner concerns, an ordinary Ref or Context.Reference for application-owned test state, and FiberSet for explicit fiber tracking.

- `TestServices.withAnnotationsScoped` -> `none`: The annotation service was removed. Use Vitest metadata/options for runner concerns, an ordinary Ref or Context.Reference for application-owned test state, and FiberSet for explicit fiber tracking.

- `TestServices.withLiveScoped` -> `none`: There is no scoped TestLive service override. Apply TestClock.withLive to a specific effect, or choose it.live at test declaration time.

- `TestServices.withSize` -> `Effect.provideService(effect, CurrentSize, size)`: Provide a custom size Context.Reference for the duration of the wrapped effect.

- `TestServices.withSized` -> `Effect.provideService(effect, CurrentSize, size)`: Collapse the old TestSized wrapper to its numeric value and provide the custom reference.

- `TestServices.withSizedScoped` -> `Effect.updateServiceScoped(CurrentSize, () => size)`: For a scope-bounded override use updateServiceScoped; otherwise prefer wrapping the workflow with Effect.provideService.

- `TestServices.withTestConfig` -> `none`: The runner no longer reads an Effect TestConfig service. Use Vitest TestOptions and property-test arbitrary options; model application state as a custom Context.Reference.

- `TestServices.withTestConfigScoped` -> `none`: The runner no longer reads an Effect TestConfig service. Use Vitest TestOptions and property-test arbitrary options; model application state as a custom Context.Reference.
