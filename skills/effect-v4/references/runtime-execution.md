# Running Effects (Runtime Execution)

Use this guide when deciding how and where to execute effects.

Official examples:
- `references/examples/01_effect/06_running/10_run-main.ts` (`NodeRuntime.runMain` / `BunRuntime.runMain`)
- `references/examples/01_effect/06_running/20_layer-launch.ts` (`Layer.launch`)
- `references/examples/04_integration/10_managed-runtime.ts` (`ManagedRuntime` + Hono)

## Rules

- Effects are descriptions; `run*` executes them. Call `run*` only at the edge:
  process entrypoint, framework handler bridge, test harness. Never in library
  or service code.
- `R` must be `never` before running: provide layers first.
- For processes, use the platform `runMain` (signals, exit code, error log).
  For embedding in a non-Effect host (HTTP framework, UI, callbacks), use
  `ManagedRuntime`.

## Entry points: `runMain`

```ts
import { NodeRuntime } from "@effect/platform-node" // or BunRuntime from @effect/platform-bun,
import { Effect, Layer } from "effect"              //    DenoRuntime from @effect/platform-deno,
                                                    //    BrowserRuntime from @effect/platform-browser

const program = Effect.gen(function*() {
  yield* Effect.log("hello")
}).pipe(Effect.provide(AppLayer))

NodeRuntime.runMain(program)
// options: { disableErrorReporting?: boolean, teardown?: Runtime.Teardown }
NodeRuntime.runMain(program, { disableErrorReporting: true })
```

Node/Bun/Deno `runMain`:
- forks the program, installs `SIGINT`/`SIGTERM` handlers that interrupt the
  main fiber (finalizers and layer scopes run), then exits with the teardown's code;
- logs unreported failures (unless `disableErrorReporting`);
- keeps the process alive while the main fiber runs.

Exit codes via `Runtime.defaultTeardown`: `0` success, `130` interrupt-only,
otherwise the squashed error's `[Runtime.errorExitCode]` or `1`.

```ts
import { Data, Runtime } from "effect"

class ConfigMissing extends Data.TaggedError("ConfigMissing")<{}> {
  readonly [Runtime.errorExitCode] = 78 // custom exit code
  readonly [Runtime.errorReported] = false // already reported; skip the default log
}
```

## Long-running apps: `Layer.launch`

Model the whole app (HTTP server, workers) as layers; `Layer.launch` builds it
and waits forever. Interruption (e.g. Ctrl-C via `runMain`) closes the layer scope.

```ts
import { NodeHttpServer, NodeRuntime } from "@effect/platform-node"
import { Effect, Layer } from "effect"
import { HttpRouter, HttpServerResponse } from "effect/http"
import { createServer } from "node:http"

const Routes = HttpRouter.use(Effect.fn(function*(router) {
  yield* router.add("GET", "/health", Effect.succeed(HttpServerResponse.text("ok")))
}))

const HttpLive = HttpRouter.serve(Routes).pipe(
  Layer.provide(NodeHttpServer.layer(createServer, { port: 3000 }))
)

NodeRuntime.runMain(Layer.launch(HttpLive))
```

Background-only processes: `Layer.effectDiscard(... Effect.forkScoped)` +
`Layer.launch` (see `layer-patterns.md`).

## `Effect.run*`

| Runner | Returns | Use |
|---|---|---|
| `Effect.runPromise(e, opts?)` | `Promise<A>`; rejects with the squashed error (your error value, not a wrapper) | async edge |
| `Effect.runPromiseExit(e, opts?)` | `Promise<Exit<A, E>>` | need full failure info |
| `Effect.runSync(e)` | `A`; throws on failure, throws `AsyncFiberError` if async | sync-only code |
| `Effect.runSyncExit(e)` | `Exit<A, E>` | sync, inspect failure |
| `Effect.runFork(e, opts?)` | `Fiber<A, E>` | start in background; `Fiber.join`/`interrupt` |
| `Effect.runCallback(e, { onExit })` | interruptor function | callback-style hosts |

`RunOptions`: `{ signal?: AbortSignal, scheduler?, uninterruptible?, onFiberStart? }`.
An aborted `signal` interrupts the fiber.

`*With(context)` variants run effects that still need services you already
hold: `Effect.runPromiseWith(ctx)(e)`, `runForkWith`, `runSyncWith`,
`runSyncExitWith`, `runPromiseExitWith`, `runCallbackWith`. Capture `ctx` with
`yield* Effect.context<R>()`.

```ts
import { Cause, Effect, Exit } from "effect"

const exit = await Effect.runPromiseExit(program)
if (Exit.isFailure(exit)) console.error(Cause.pretty(exit.cause))
```

## Framework integration: `ManagedRuntime`

```ts
import { Effect, Layer, ManagedRuntime } from "effect"

// Share one memo map if you create several runtimes from overlapping layers
export const appMemoMap = Layer.makeMemoMapUnsafe()
export const runtime = ManagedRuntime.make(TodoRepo.layer, { memoMap: appMemoMap })

app.get("/todos", async (c) => {
  const todos = await runtime.runPromise(TodoRepo.use((repo) => repo.getAll))
  return c.json(todos)
})

const shutdown = () => void runtime.dispose()
process.once("SIGINT", shutdown)
process.once("SIGTERM", shutdown)
```

- Layer is built lazily on first run and cached; `dispose()` / `disposeEffect`
  / `await using runtime = ManagedRuntime.make(...)` (`Symbol.asyncDispose`)
  interrupts fibers started through the runtime, waits for them, then releases
  layer resources. A disposed runtime cannot be reused.
- Methods: `runPromise`, `runPromiseExit`, `runSync`, `runSyncExit`, `runFork`,
  `runCallback`, `context()` (Promise), `contextEffect`.
- Handle expected errors inside the effect (`Effect.catchTag(...)`) and map to
  HTTP responses there; don't rely on rejected promises.
- Create one runtime per app (module scope), not per request.

## The `Runtime` module in v4

`Runtime<R>`, `Runtime.runFork(runtime)`, `Effect.runtime<R>()`,
`RuntimeFlags`, `Layer.toRuntime` are gone. `Runtime` now only contains process
lifecycle helpers:

- `Runtime.makeRunMain(({ fiber, teardown }) => ...)` — build a custom `runMain`
- `Runtime.Teardown`, `Runtime.defaultTeardown`
- `Runtime.errorExitCode` / `getErrorExitCode`, `Runtime.errorReported` / `getErrorReported`

Use `Context<R>` where v3 code passed a `Runtime<R>` (`references/migration/runtime.md`).

## Keep-alive (corrected for 4.0.1)

Only `runMain` (via `Runtime.makeRunMain`) keeps the host process alive while
the main fiber runs. Plain `Effect.runPromise` / `runFork` do **not**: a
program suspended on something that isn't a host timer/IO handle (e.g. only
`Deferred.await`, `Effect.never`, a `Queue.take` with no producer) lets Node
exit with the promise unresolved. Fiber-level keep-alive timers existed in early
v4 betas and were removed; `references/migration/fiber-keep-alive.md` describes
the older behavior. Use `runMain` for process entrypoints.

## Pitfalls

- `run*` inside services/library code (breaks composition, loses context/tracing).
- `runSync` on effects that sleep, await promises, or use async layers.
- Creating a `ManagedRuntime` per request (rebuilds layers each time).
- Forgetting `runtime.dispose()` on shutdown (leaks pools/sockets).
- `effect/unstable/...` or `@effect/platform` imports in entrypoints; use
  `effect/http` etc. and the `@effect/platform-*` package for your runtime.
