# Troubleshooting (Effect 4.0.x)

Use this guide when code fails to compile, fails at runtime, or behaves
unexpectedly. Most v4 problems are leftovers from v3 or from v4 betas.

## Setup and packaging

| Symptom | Cause | Fix |
|---|---|---|
| `Cannot find module 'effect/unstable/http'` (any `effect/unstable/*`) | Beta-era paths were removed in 4.0 stable, with no compat exports | Import from `effect/<area>`: `effect/http`, `effect/http-api`, `effect/rpc`, `effect/sql`, `effect/cli`, `effect/ai`, `effect/cluster`, `effect/workflow`, `effect/observability`, `effect/schema`, `effect/socket`, `effect/process`, `effect/persistence`, `effect/reactivity`, `effect/encoding`, `effect/net`, `effect/eventlog`, `effect/workers`, `effect/devtools`, `effect/testing` (barrel or `effect/http/HttpClient`) |
| `effect/httpapi` not found | Renamed | `effect/http-api` |
| `@effect/platform`, `@effect/rpc`, `@effect/sql`, `@effect/cli`, `@effect/ai`, `@effect/cluster`, `@effect/workflow`, `@effect/experimental` imports | Merged into `effect` | Use `effect/<area>`; keep only `@effect/platform-node\|bun\|deno\|browser`, `@effect/sql-*`, `@effect/ai-*`, `@effect/opentelemetry`, `@effect/atom-*`, `@effect/vitest` |
| Duplicate-type errors (`Effect<...>` not assignable to `Effect<...>`), two copies of `effect` in `node_modules` | Version mismatch | All Effect packages share one version: pin `effect` and every `@effect/*` to the same `4.0.x` and dedupe |
| Odd inference errors, `const` type params ignored | Old TypeScript | TS >= 5.9 required (TS 7 recommended) |
| `@effect/vitest` peer error | Vitest version | Needs Vitest 5 |
| `Micro` import fails | `Micro` removed in v4 | Use `Effect` |
| `@effect/sql-sqlite-node` fails to load | Uses `node:sqlite` | Node >= 22.16 |

## v3 / beta leftovers (compile errors)

| Leftover | v4 |
|---|---|
| `ServiceMap.*` (early betas) | `Context.*` |
| `Context.Tag`, `Effect.Tag`, `Effect.Service`, `X.Default` | `Context.Service<Self, Shape>()("id")` + `static layer` (`dependency-management.md`) |
| `FiberRef`, `Effect.locally`, `References.CurrentConcurrency`, `Effect.withConcurrency` | `Context.Reference`, `Effect.provideService`, explicit `concurrency` (`concurrency-advanced.md`) |
| `Either`, `Effect.either` | `Result`, `Effect.result` |
| `Effect.catchAll`, `Effect.catchAllCause` | `Effect.catch`, `Effect.catchCause` |
| `Effect.fork`, `Effect.forkDaemon` | `Effect.forkChild`, `Effect.forkDetach` |
| `Effect.makeSemaphore`, `Effect.makeLatch` | `Semaphore.make`, `Latch.make` |
| `Layer.scoped`, `Layer.scopedDiscard`, `Layer.unwrapEffect` | `Layer.effect`, `Layer.effectDiscard`, `Layer.unwrap` |
| `Scope.extend` | `Scope.provide` |
| `Runtime<R>`, `Effect.runtime`, `Runtime.runFork` | `Context<R>`, `Effect.context`, `Effect.runForkWith` |
| `STM`, `TRef`, ... | `Effect.tx`, `TxRef`, ... |
| `@effect/schema` imports | `Schema` from `effect` (and `effect/schema` for advanced modules) |
| `TestSchema.verifyLosslessTransformation` | `TestSchema.verifyRoundTrip` |

Full per-module mappings: `references/migration/api-map/<module>.md`.

## Behavior changes that compile but break

- **Tuple order flip (4.0.0):** `Effect.partition`, `Array.partition`,
  `Chunk.partition`, `Record.partition`, their `separate` helpers, and
  `Option.partitionMap` now return `[successes, failures]` / `[passes, fails]`.
  Code written for v3 or the betas (`[failures, successes]`) silently swaps the halves.
- **Not Effects anymore:** `Option`, `Result`, `Ref`, `Deferred`, `Fiber` can't
  be `yield*`ed or passed to Effect combinators. Use `Effect.fromOption`,
  `Effect.fromResult`, `Ref.get`, `Deferred.await`, `Fiber.join`. Services and
  `Config` are still yieldable (`references/migration/yieldable.md`).
- **`Schema.brand`** accepts a single identifier and is type-only (4.0.0);
  chain `brand` calls for multiple brands.
- **Process exits early** with a pending `Effect.runPromise`: core fibers do not
  keep Node alive; use `NodeRuntime.runMain` (`runtime-execution.md`).
- **Layer built twice:** sibling `Effect.provide` calls don't share memoization;
  compose into one layer and provide once (`layer-patterns.md`).
- **`Scope.close` type error on `yield* Effect.scope`:** closing needs a
  `Scope.Closeable` from `Scope.make`/`Scope.fork`.
- **`Queue.State.takers`** entries are `Queue.Taker` objects: call `entry.resume(...)`.

## Effect code mistakes

- **Missing `return` before failing in a generator.** `yield* new MyError(...)`
  without `return` lets TypeScript think execution continues (wrong return type,
  "possibly undefined" errors). Always `return yield* new MyError({...})`.
- **`.pipe` on `Effect.fn`.** `Effect.fn("name")(function*() {...}).pipe(...)`
  throws `pipe is not a function` (the result is a plain function). Pass
  combinators as extra arguments:
  `Effect.fn("name")(function*() {...}, Effect.catchTag("E", h), Effect.withSpan(...))`.
- **Wrapping `Effect.gen` in a function** instead of `Effect.fn` /
  `Effect.fnUntraced` — loses span/stack info.
- **Services requiring services in method signatures** — resolve deps in the
  layer so methods have `R = never`.
- **`run*` inside services or library code.**
- **Throwing in `Effect.sync`/`Effect.gen`** for expected failures — becomes a
  defect. Use typed errors (`Schema.TaggedError` / `Data.TaggedError`) or
  `Effect.try({ try, catch })`.
- **Manual `typeof` / `JSON.parse` validation** — decode with `Schema`.
- **`Date`/`Date.now()`** — use `DateTime` / `Clock` (testable with `TestClock`).

## Runtime error messages

| Message | Meaning |
|---|---|
| `Service not found: <key>` (defect) | A service was accessed without being provided — usually a cast or an unsafe `Context.getUnsafe`; check `R` and the layer graph |
| `AsyncFiberError: An asynchronous Effect was executed with Effect.runSync` | Use `runPromise` or remove async steps |
| `UnimplementedError: <key>: Unimplemented method "x"` | A `Layer.mock` stub was called for a member you didn't provide |
| `TimeoutError` | `Effect.timeout` elapsed |
| `NoSuchElementError` | `Effect.fromOption` on `None`, or similar |
| `ExceededCapacityError` | `RcMap` with `capacity` is full |

`Effect.runPromise` rejects with the squashed error value itself (your
`TaggedError` instance), not a wrapper.

## Diagnostics

```ts
import { Cause, Effect, Exit } from "effect"

// Full cause (failures, defects, interrupts) as a readable string
const debugged = program.pipe(
  Effect.tapCause((cause) => Effect.logError(Cause.pretty(cause)))
)

// Inspect the Exit at the edge
const exit = await Effect.runPromiseExit(program)
if (Exit.isFailure(exit)) {
  console.error(Cause.pretty(exit.cause))
  // exit.cause.reasons: flat list of Fail / Die / Interrupt reasons
}

// Recover from everything (including defects) for a boundary
const safe = program.pipe(Effect.catchCause((cause) => Effect.succeed(Cause.pretty(cause))))
```

- Inspect the inferred `Effect<A, E, R>` type first; unexpected `R` means a
  missing layer, unexpected `E` means an unhandled error channel.
- `Cause.hasFails`, `Cause.hasDies`, `Cause.hasInterruptsOnly`,
  `Cause.findError`, `Cause.squash` help classify failures
  (`references/exit-cause.md`).
- Add `Effect.withSpan` / `Effect.fn("name")` and logs (`Effect.log`,
  `Effect.annotateLogs`) to locate failures; set
  `References.MinimumLogLevel` to `"Debug"` for more output.

## Agent debug workflow

1. Check imports: no `effect/unstable/*`, no merged `@effect/*` packages, one
   Effect version everywhere.
2. Confirm the program is executed (`run*` / `runMain` / `Layer.launch` at an entrypoint or test).
3. Inspect `R`; ensure every service is provided by a composed layer.
4. Decide whether the failure is expected (`E`) or a defect/interrupt (`Cause`).
5. For concurrency issues, check fiber ownership (scope, parent, FiberSet) and
   interruption paths.
6. For boundary inputs, confirm Schema decoding happens and inspect the issue output.
