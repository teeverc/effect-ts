# v3 → v4 API map: @effect/vitest/index, @effect/vitest/utils

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/vitest/index`

- `index.ApiConfig` -> `vitest/node#ApiConfig`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.ArgumentsType` -> `T extends (...args: infer A) => any ? A : never`: Vitest 3 marked this root alias as an internal helper. Define the small TypeScript shape locally instead of depending on transitive internals.

- `index.Arrayable` -> `T | Array<T>`: Vitest 3 marked this root alias as an internal helper. Define the small TypeScript shape locally instead of depending on transitive internals.

- `index.Assertion` -> `vitest#Assertion`: Vitest 5 takes the matcher return type first. Replace Assertion\<T\> with Assertion\<void, T\> or Assertion\<Promise\<void\>, T\> for asynchronous assertions.

- `index.Awaitable` -> `T | PromiseLike<T>`: Vitest 3 marked this root alias as an internal helper. Define the small TypeScript shape locally instead of depending on transitive internals.

- `index.BaseCoverageOptions` -> `vitest/node#BaseCoverageOptions`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.BenchFactory` -> `vitest#Bench`: Use the Vitest 5 test-context bench fixture type. It is no longer the tinybench factory constructor.

- `index.BenchFunction` -> `vitest#BenchFn`: Use BenchFn for the callback passed to the Vitest 5 test-context bench fixture.

- `index.BenchTask` -> `vitest#BenchRegistration`: Migrate to a fixture registration and await its run() method; review its fields instead of treating it as a tinybench task.

- `index.BenchTaskResult` -> `vitest#BenchResult`: Use the result returned by awaiting the Vitest 5 fixture registration's run() method.

- `index.Benchmark` -> `vitest#TestBenchmark`: Use TestBenchmark for recorded benchmark data on a test; benchmarks are no longer standalone test tasks.

- `index.BenchmarkAPI` -> `vitest#Bench`: Use the test-context bench fixture. Move skip, only, and todo to the enclosing test.

- `index.BenchmarkResult` -> `vitest#BenchResult`: Use the result returned by awaiting the Vitest 5 fixture registration's run() method; review its changed fields.

- `index.BenchmarkUserOptions` -> `vitest/node#BenchmarkUserOptions`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.BrowserConfigOptions` -> `vitest/node#BrowserConfigOptions`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.BrowserScript` -> `vitest/node#BrowserScript`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.BuiltinEnvironment` -> `vitest/node#BuiltinEnvironment`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.CSSModuleScopeStrategy` -> `vitest/node#CSSModuleScopeStrategy`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.CollectLineNumbers` -> `vitest/node#TypeCheckCollectLineNumbers`: Vitest 3 deprecated the root alias in favor of this renamed vitest/node type.

- `index.CollectLines` -> `vitest/node#TypeCheckCollectLines`: Vitest 3 deprecated the root alias in favor of this renamed vitest/node type.

- `index.Constructable` -> `new (...args: any[]) => any`: Vitest 3 marked this root alias as an internal helper. Define the small TypeScript shape locally instead of depending on transitive internals.

- `index.Context` -> `vitest/node#TypeCheckContext`: Vitest 3 deprecated the root alias in favor of this renamed vitest/node type.

- `index.CoverageIstanbulOptions` -> `vitest/node#CoverageIstanbulOptions`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.CoverageOptions` -> `vitest/node#CoverageOptions`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.CoverageProvider` -> `vitest/node#CoverageProvider`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.CoverageProviderModule` -> `vitest/node#CoverageProviderModule`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.CoverageReporter` -> `vitest/node#CoverageReporter`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.CoverageV8Options` -> `vitest/node#CoverageV8Options`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.Custom` -> `vitest#RunnerTestCase`: Vitest 4 removed the deprecated unprefixed runner alias. Import the explicit Runner\* type from vitest.

- `index.CustomProviderOptions` -> `vitest/node#CustomProviderOptions`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.DepsOptimizationOptions` -> `vitest/node#DepsOptimizationOptions`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.DoneCallback` -> `none`: Vitest does not support callback-style tests. Return a Promise or, in @effect/vitest tests, return an Effect.

- `index.Environment` -> `vitest/runtime#Environment`: This was a deprecated root re-export. Import it from vitest/runtime; Vitest 5 exposes custom environments through vitest/runtime.

- `index.EnvironmentOptions` -> `vitest/node#EnvironmentOptions`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.EnvironmentReturn` -> `vitest/runtime#EnvironmentReturn`: This was a deprecated root re-export. Import it from vitest/runtime; Vitest 5 exposes custom environments through vitest/runtime.

- `index.ErrorWithDiff` -> `vitest#TestError`: Vitest 3 deprecated ErrorWithDiff in favor of TestError; review the tightened actual, expected, and cause fields.

- `index.ExpectPollOptions` -> `NonNullable<Parameters<typeof import("vitest").expect.poll>[1]>`: Vitest 5 removes the named options export; derive the options from the public expect.poll function.

- `index.ExtendedContext` -> `vitest#TestContext`: The separate context alias was removed. Vitest 5 uses TestContext, which includes the current task and lifecycle methods.

- `index.File` -> `vitest#RunnerTestFile`: Vitest 4 removed the deprecated unprefixed runner alias. Import the explicit Runner\* type from vitest.

- `index.HappyDOMOptions` -> `NonNullable<import("vitest/node").EnvironmentOptions["happyDOM"]>`: Vitest 5 keeps this shape only as a property of EnvironmentOptions; derive it from the public vitest/node type.

- `index.HookCleanupCallback` -> `none`: No named Vitest 5 export replaces this alias. Let the hook return type infer, or type the cleanup function locally.

- `index.HookListener` -> `none`: Infer the callback from the public hook function, or derive it with Parameters\<typeof import("vitest").beforeAll\>[0] and the corresponding hook name.

- `index.InlineConfig` -> `vitest/node#InlineConfig`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.JSDOMOptions` -> `NonNullable<import("vitest/node").EnvironmentOptions["jsdom"]>`: Vitest 5 keeps this shape only as a property of EnvironmentOptions; derive it from the public vitest/node type.

- `index.Matchers` -> `vitest#Matchers`: Augment vitest.Matchers\<R, T\> for custom matchers. R is the matcher return type and T is the received value; @vitest/expect no longer shares Vitest's assertion state.

- `index.Mock` -> `vitest#Mock`: This was a Vitest re-export, not Effect API. Import it directly from vitest; @effect/vitest/index is not a valid v4 route.

- `index.ModuleCache` -> `none`: Vitest 3 marked this unused internal cache shape deprecated; Vitest 5 has no public replacement.

- `index.MutableArray` -> `{ -readonly [K in keyof T]: T[K] }`: Vitest 3 marked this root alias as an internal helper. Define the small TypeScript shape locally instead of depending on transitive internals.

- `index.Nullable` -> `T | null | undefined`: Vitest 3 marked this root alias as an internal helper. Define the small TypeScript shape locally instead of depending on transitive internals.

- `index.Pool` -> `vitest/node#Pool`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.PoolOptions` -> `vitest/config#TestUserConfig`: The v3 built-in poolOptions object was removed. Move its fields to Vitest 5 top-level config such as maxWorkers and vmMemoryLimit; vitest/node PoolOptions is a different custom-pool API.

- `index.ProjectConfig` -> `vitest/node#ProjectConfig`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.RawErrsMap` -> `vitest/node#TypeCheckRawErrorsMap`: Vitest 3 deprecated the root alias in favor of this renamed vitest/node type.

- `index.ReportContext` -> `vitest/node#ReportContext`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.Reporter` -> `vitest/node#Reporter`: Import Reporter from vitest/node; the deprecated vitest/reporters entrypoint was removed in Vitest 5.

- `index.ResolveIdFunction` -> `none`: This deprecated vite-node callback was removed. Use Vite environment or module-runner APIs.

- `index.ResolvedConfig` -> `vitest/node#ResolvedConfig`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.ResolvedCoverageOptions` -> `vitest/node#ResolvedCoverageOptions`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.ResolvedTestEnvironment` -> `none`: Vitest 3 marked this type unsupported. Use Environment from vitest/runtime for custom environments.

- `index.RootAndTarget` -> `vitest/node#TypeCheckRootAndTarget`: Vitest 3 deprecated the root alias in favor of this renamed vitest/node type.

- `index.RunnerCustomCase` -> `vitest#RunnerTestCase`: Vitest 4 removed the deprecated unprefixed runner alias. Import the explicit Runner\* type from vitest.

- `index.RuntimeContext` -> `none`: Vitest 5 deprecates @vitest/runner and does not expose this internal state type. Extend TestRunner from vitest and use its public methods instead.

- `index.SequenceHooks` -> `vitest/node#SequenceHooks`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.SequenceSetupFiles` -> `vitest/node#SequenceSetupFiles`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.SerializableSpec` -> `vitest#SerializedTestSpecification`: Use the non-deprecated Vitest name; SerializableSpec was only an alias.

- `index.Suite` -> `vitest#RunnerTestSuite`: Vitest 4 removed the deprecated unprefixed runner alias. Import the explicit Runner\* type from vitest.

- `index.SuiteHooks` -> `ReturnType<typeof import("vitest").TestRunner.getSuiteHooks>`: Derive the hook collection from Vitest 5's public TestRunner API; ordinary tests should use public hook functions.

- `index.Task` -> `vitest#RunnerTask`: Vitest 4 removed the deprecated unprefixed runner alias. Import the explicit Runner\* type from vitest.

- `index.TaskBase` -> `vitest#RunnerTaskBase`: Vitest 4 removed the deprecated unprefixed runner alias. Import the explicit Runner\* type from vitest.

- `index.TaskContext` -> `vitest#TestContext`: The separate context alias was removed. Vitest 5 uses TestContext, which includes the current task and lifecycle methods.

- `index.TaskResult` -> `vitest#RunnerTaskResult`: Vitest 4 removed the deprecated unprefixed runner alias. Import the explicit Runner\* type from vitest.

- `index.TaskResultPack` -> `vitest#RunnerTaskResultPack`: Vitest 4 removed the deprecated unprefixed runner alias. Import the explicit Runner\* type from vitest.

- `index.Test` -> `vitest#RunnerTestCase`: Vitest 4 removed the deprecated unprefixed runner alias. Import the explicit Runner\* type from vitest.

- `index.TransformModePatterns` -> `none`: This was removed with vite-node transform modes. Configure the Vite environment and its dependency optimizer instead.

- `index.TscErrorInfo` -> `vitest/node#TypeCheckErrorInfo`: Vitest 3 deprecated the root alias in favor of this renamed vitest/node type.

- `index.TypecheckConfig` -> `vitest/node#TypecheckConfig`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.UserConfig` -> `vitest/config#TestUserConfig`: Vitest 5 exposes its config as TestUserConfig; ViteUserConfig is the separate Vite configuration type.

- `index.UserWorkspaceConfig` -> `vitest/config#UserWorkspaceConfig`: Import the type from vitest/config and migrate Vitest workspace configuration to projects.

- `index.VitestEnvironment` -> `vitest/node#VitestEnvironment`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.VitestRunMode` -> `vitest/node#VitestRunMode`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.VmEnvironmentReturn` -> `vitest/runtime#VmEnvironmentReturn`: This was a deprecated root re-export. Import it from vitest/runtime; Vitest 5 exposes custom environments through vitest/runtime.

- `index.WorkerContext` -> `vitest/node#WorkerContext`: This was a deprecated Vitest 3 root re-export. Import the type directly from vitest/node and review its Vitest 5 shape.

- `index.WorkerRPC` -> `none`: The concrete worker RPC composition is internal. Use public Vitest RuntimeRPC, RunnerRPC, ContextRPC, or WorkerRequest types only when their narrower contract fits.

- `index.bench` -> `vitest#test`: Vitest 5 removes the top-level bench export. Destructure bench from a regular test's context and await bench(name, fn).run(); use skip, only, or todo on the enclosing test.

- `index.chai.Should` -> `vitest#chai.Should`: This was a Vitest re-export, not Effect API. Import it directly from vitest; @effect/vitest/index is not a valid v4 route.

- `index.describe` -> `vitest#describe`: Vitest 5 removes describe.sequential and sequential options. Use describe(name, { concurrent: false }, body) for suites that depend on ordering.

- `index.expect` -> `vitest#expect`: This was a Vitest re-export, not Effect API. Import it directly from vitest; @effect/vitest/index is not a valid v4 route.

- `index.it` -> `@effect/vitest#it`: Effect helpers retain their calling convention. Vitest 5 removes it.sequential; pass { concurrent: false } to the native test or as the Effect helper's third argument.

- `index.scoped` -> `@effect/vitest#effect`: V4 effect tests are scoped and provide the test environment. Replace scoped(...) with effect(...), and it.scoped(...) with it.effect(...).

- `index.scopedLive` -> `@effect/vitest#live`: V4 live tests are scoped automatically. Replace scopedLive(...) with live(...), and it.scopedLive(...) with it.live(...).

- `index.should` -> `vitest#should`: This was a Vitest re-export, not Effect API. Import it directly from vitest; @effect/vitest/index is not a valid v4 route.

- `index.test` -> `vitest#test`: Vitest 5 removes test.sequential and sequential options. Pass { concurrent: false } to opt out of inherited concurrency.

## `@effect/vitest/utils`

- `utils.assertFailure` -> `assertExitFailure`: For v3 Exit values, rename to assertExitFailure. In v4, assertFailure instead asserts Result.Failure.

- `utils.assertLeft` -> `assertFailure`: Either became Result in v4: migrate Left to Result.Failure, then use assertFailure; narrowed payload access changes from .left to .failure.

- `utils.assertMatch` -> `assertMatch`: Unchanged positional helper and behavior; only the parameter spelling changed, so call sites need no change.

- `utils.assertRight` -> `assertSuccess`: Either became Result in v4: migrate Right to Result.Success, then use assertSuccess; narrowed payload access changes from .right to .success.

- `utils.assertSuccess` -> `assertExitSuccess`: For v3 Exit values, rename to assertExitSuccess. In v4, assertSuccess instead asserts Result.Success.
