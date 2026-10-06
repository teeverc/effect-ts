# Concurrency Advanced (References, Semaphore, Latch, Deferred, Refs)

Use this guide for fiber-local/ambient state and coordination primitives.
Forking, racing, and fiber collections are in `references/concurrency.md`;
`Queue`, `PubSub`, and `Effect.tx`/`Tx*` in `references/streams-queues-stm.md`.

## Context.Reference replaces FiberRef

`FiberRef`, `FiberRefs`, `FiberRefsPatch`, `Effect.locally*` are removed.
Ambient, fiber-inherited values are `Context.Reference`s: services with a
default value. Reading one never adds to `R`; providing one scopes the value to
that effect **and the fibers it forks**.

```ts
import { Context, Effect, Layer } from "effect"

export const RequestId = Context.Reference<string | undefined>("myapp/RequestId", {
  defaultValue: () => undefined
})

const handler = Effect.gen(function*() {
  const id = yield* RequestId // read
  yield* Effect.log(`request ${id}`)
})

handler.pipe(Effect.provideService(RequestId, "req-1"))              // v3 Effect.locally
handler.pipe(Effect.updateService(RequestId, (id) => id ?? "anon"))   // v3 Effect.locallyWith
Effect.updateServiceScoped(RequestId, () => "req-2")                  // until current Scope closes
Layer.succeed(RequestId, "req-3")                                     // as a layer
```

There is no `FiberRef.set` equivalent that mutates state for the rest of the
current fiber; scope the value with `provideService`/`updateServiceScoped`, or
use a `Ref` (shared mutable) when you truly need mutation. Read a reference from
outside the fiber with `fiber.getRef(ref)`.

## Built-in references (`effect/References`)

| Reference | Type / default | v3 FiberRef |
|---|---|---|
| `References.MinimumLogLevel` | `LogLevel`, `"Info"` | `currentMinimumLogLevel` |
| `References.CurrentLogLevel` | `Severity`, `"Info"` | `currentLogLevel` |
| `References.CurrentLogAnnotations` | record | `currentLogAnnotations` |
| `References.CurrentLogSpans` | `[label, ts][]` | `currentLogSpan` |
| `References.CurrentLoggers` | `ReadonlySet<Logger>` | `currentLoggers` |
| `References.LogToStderr` | `boolean` | — |
| `References.UnhandledLogLevel` | `Severity \| undefined` | `unhandledErrorLogLevel` |
| `References.TracerEnabled` | `boolean` | `currentTracerEnabled` |
| `References.TracerTimingEnabled`, `TracerSpanAnnotations`, `TracerSpanLinks` | | `currentTracerTimingEnabled`, `currentTracerSpan*` |
| `References.Tracer`, `CurrentTraceLevel`, `MinimumTraceLevel`, `DisablePropagation` | (re-exported from `Tracer`) | — |
| `References.Scheduler`, `MaxOpsBeforeYield`, `PreventSchedulerYield` | (re-exported from `Scheduler`) | `currentScheduler`, `currentMaxOpsBeforeYield` |
| `References.CurrentStackFrame` | diagnostics | — |

`References.CurrentConcurrency` and `Effect.withConcurrency` were **removed**
before 4.0 stable: pass `concurrency` to each combinator. Other modules expose
their own references (`Clock.Clock`, `Console.Console`, `Random.Random`,
`Metric.CurrentMetricAttributes`, `SqlClient.SafeIntegers`, ...).

```ts
import { Effect, Layer, References } from "effect"

program.pipe(Effect.provideService(References.MinimumLogLevel, "Debug"))
const DebugLogs = Layer.succeed(References.MinimumLogLevel, "Debug")
```

Mapping table and details: `references/migration/fiberref.md`,
`references/migration/api-map/effect-FiberRef.md`.

## Semaphore (`effect/Semaphore`)

```ts
import { Effect, Semaphore } from "effect"

const program = Effect.gen(function*() {
  const sem = yield* Semaphore.make(3) // v3: Effect.makeSemaphore(3)

  // Limit concurrent access
  yield* Effect.forEach(urls, (url) => fetchUrl(url).pipe(sem.withPermit), {
    concurrency: "unbounded"
  })

  yield* Semaphore.withPermits(sem, 2, heavyJob)             // module form
  const maybe = yield* sem.withPermitsIfAvailable(1)(quickJob) // Option<A>, no waiting

  // Manual control (pair take/release yourself; prefer withPermit*)
  yield* Semaphore.take(sem, 1)
  const ok = yield* Semaphore.takeIfAvailable(sem, 1) // boolean
  yield* Semaphore.release(sem, 1)
  yield* Semaphore.resize(sem, 5)
})
```

`Semaphore.makeUnsafe(n)` for synchronous construction (e.g. in a class field).
Use one mutex `Semaphore.make(1)` to serialize effectful updates when a
`SynchronizedRef` is not a fit.

### PartitionedSemaphore

A shared permit pool where waiters are grouped by key and released permits
are distributed round-robin across partitions (fairness between tenants).

```ts
import { PartitionedSemaphore } from "effect"

const sem = yield* PartitionedSemaphore.make<string>({ permits: 10 })
yield* PartitionedSemaphore.withPermit(sem, tenantId, handleRequest)
yield* PartitionedSemaphore.withPermits(sem, tenantId, 3, bulkJob)
const free = yield* PartitionedSemaphore.available(sem)
```

## Latch (`effect/Latch`)

A gate: fibers `await` until it is opened. v3 `Effect.makeLatch` → `Latch.make`.

```ts
import { Effect, Latch } from "effect"

const program = Effect.gen(function*() {
  const ready = yield* Latch.make() // closed; Latch.make(true) starts open
  yield* Effect.forkChild(Latch.await(ready).pipe(Effect.andThen(worker)))
  yield* warmUp
  yield* Latch.open(ready)         // releases current and future waiters
  // Latch.close(ready)            // block future waiters again
  // Latch.release(ready)          // release current waiters, stay closed
  // Latch.whenOpen(ready, effect) // run effect once open
})
```

Methods also exist on the instance: `latch.open`, `latch.await`,
`latch.close`, `latch.release`, `latch.whenOpen(effect)`, `latch.isOpen()`.

## Deferred (`effect/Deferred`)

One-shot promise between fibers.

```ts
import { Deferred, Effect, Fiber } from "effect"

const program = Effect.gen(function*() {
  const d = yield* Deferred.make<number, string>()
  const fiber = yield* Effect.forkChild(Deferred.await(d))
  yield* Deferred.succeed(d, 42) // also: fail, die, interrupt, done(exit),
                                 // complete(effect): run once, store result
  return yield* Fiber.join(fiber)
})
```

`Deferred` is not an `Effect` in v4 — use `Deferred.await(d)`, not `yield* d`.
`Deferred.poll(d)` / `Deferred.isDone(d)` inspect without waiting.

## Ref, SynchronizedRef, SubscriptionRef

```ts
import { Effect, Ref, SubscriptionRef, SynchronizedRef } from "effect"

const counter = yield* Ref.make(0)
yield* Ref.update(counter, (n) => n + 1)
const next = yield* Ref.modify(counter, (n) => [n + 1, n + 1] as const)
const value = yield* Ref.get(counter) // NOT `yield* counter` in v4

// Effectful updates, serialized
const cache = yield* SynchronizedRef.make(new Map<string, string>())
yield* SynchronizedRef.updateEffect(cache, (m) =>
  fetchValue("k").pipe(Effect.map((v) => new Map(m).set("k", v)))
)

// Observable state: changes is a Stream of current + subsequent values
const state = yield* SubscriptionRef.make({ count: 0 })
yield* SubscriptionRef.changes(state).pipe(
  Stream.runForEach((s) => Effect.log(s.count)),
  Effect.forkScoped
)
yield* SubscriptionRef.update(state, (s) => ({ count: s.count + 1 }))
```

- `SynchronizedRef` is no longer typed as a subtype of `Ref`: use the
  `SynchronizedRef.*` functions, not `Ref.*`, on it.
- `Ref`, `Deferred`, `Fiber`, `Option`, `Result` are no longer yieldable;
  services (`Context.Service`) and `Config` still are
  (`references/migration/yieldable.md`).

## Effect.tx (STM) in brief

Software transactional memory is built in: wrap reads/writes of `TxRef`,
`TxQueue`, `TxHashMap`, `TxSemaphore`, ... in `Effect.tx(...)`; nested `tx`
calls join the outer transaction; `Effect.txRetry` blocks until a read value
changes. There is no separate `STM` type. See `references/streams-queues-stm.md`.

```ts
import { Effect, TxRef } from "effect"

const transfer = (from: TxRef.TxRef<number>, to: TxRef.TxRef<number>, n: number) =>
  Effect.tx(Effect.gen(function*() {
    const balance = yield* TxRef.get(from)
    if (balance < n) return yield* Effect.txRetry // wait for funds
    yield* TxRef.set(from, balance - n)
    yield* TxRef.update(to, (b) => b + n)
  }))
```

## Interruption control

- `Effect.uninterruptible(e)`, `Effect.interruptible(e)`,
  `Effect.uninterruptibleMask((restore) => ...)`, `Effect.interruptibleMask`.
- `Effect.onInterrupt((interruptors) => cleanup)` — `interruptors` is a
  `ReadonlySet<number>` of fiber ids.
- Pending interrupts are preserved when an uninterruptible region fails (4.0.1).

## Pitfalls

- Importing `FiberRef`, `Effect.locally`, `References.CurrentConcurrency`,
  `Effect.withConcurrency`, `Effect.makeSemaphore`, `Effect.makeLatch` (all gone).
- Using a module-level `let` instead of a `Context.Reference` for
  request-scoped values.
- `Semaphore.take` without a guaranteed `release` (use `withPermit*`).
- Treating `SubscriptionRef.changes` as "future changes only": it starts with
  the current value.
