# Concurrency (Fibers, Forking, Racing)

Use this guide for running work concurrently, forking fibers, and racing.
Coordination primitives (Semaphore, Latch, Deferred, Ref, References) are in
`references/concurrency-advanced.md`; queues/pubsub/STM in `streams-queues-stm.md`.

## Mental model

- Effects run on fibers (lightweight virtual threads). Concurrency is
  structured: every fiber has an owner (parent fiber, a `Scope`, or a
  fiber collection) that interrupts it when the owner ends.
- Prefer high-level combinators (`Effect.all`, `Effect.forEach`, `race*`,
  `Stream`) over manual forking.
- Interruption is cooperative and runs finalizers; acquisition in
  `acquireRelease` is uninterruptible by default.

## Collection concurrency

```ts
import { Effect } from "effect"

// Sequential by default
const users = yield* Effect.forEach(ids, (id) => fetchUser(id))

// Bounded parallelism
const users2 = yield* Effect.forEach(ids, fetchUser, { concurrency: 8 })

// Fire for side effects, discard results
yield* Effect.forEach(ids, notify, { concurrency: "unbounded", discard: true })

// Tuples / iterables / structs
const { user, posts } = yield* Effect.all(
  { user: fetchUser(id), posts: fetchPosts(id) },
  { concurrency: 2 }
)

// Collect every outcome instead of short-circuiting: each element is a Result
const results = yield* Effect.all([a, b, c], { mode: "result", concurrency: 3 })

// Run everything, split outcomes. 4.0.0: returns [successes, failures]
const [ok, failed] = yield* Effect.partition(ids, fetchUser, { concurrency: 4 })
```

- `concurrency: number | "unbounded"`. The `"inherit"` option and
  `Effect.withConcurrency` (ambient concurrency) were **removed** in v4 —
  always pass the value explicitly.
- `Effect.all` `mode`: `"default"` (fail fast; interrupts siblings) or
  `"result"` (v3 `"either"`). To accumulate all failures over a collection use
  `Effect.validate(xs, f, { concurrency })` (fails with a non-empty array of errors).
- On failure with concurrency, siblings are interrupted and their cleanup
  failures are preserved in the `Cause` (4.0.0).

## Forking

| v3 | v4 | Owner / lifetime |
|---|---|---|
| `Effect.fork` | `Effect.forkChild` | parent fiber; interrupted when parent ends |
| `Effect.forkDaemon` | `Effect.forkDetach` | global; outlives the parent |
| `Effect.forkScoped` | `Effect.forkScoped` | current `Scope` (adds `Scope` to `R`) |
| `Effect.forkIn(scope)` | `Effect.forkIn(scope)` | given `Scope` |
| `Effect.forkAll` | removed — `Effect.forEach(effects, (e) => Effect.forkChild(e))` or `FiberSet` | |
| `Effect.forkWithErrorHandler` | removed — observe with `Fiber.join`/`Fiber.await` | |
| `Effect.supervised` / `Supervisor` | removed — track with `FiberSet` | |

All four accept options, data-first or data-last:

```ts
const f1 = yield* Effect.forkChild(task, { startImmediately: true })
const f2 = yield* task.pipe(Effect.forkScoped({ uninterruptible: true }))
const f3 = yield* Effect.forkIn(task, scope, { startImmediately: true })
// startImmediately?: boolean          - run synchronously up to the first async boundary
// uninterruptible?: boolean | "inherit"
```

`Effect.awaitAllChildren(effect)` delays completion until children forked by
`effect` (via `forkChild`) finish.

```ts
import { Effect, Fiber } from "effect"

const program = Effect.gen(function*() {
  const fiber = yield* Effect.forkChild(longTask)
  yield* doOtherWork
  return yield* Fiber.join(fiber) // propagates the fiber's failure
})

// Background worker bound to a scope (e.g. inside Layer.effect)
const worker = Effect.gen(function*() {
  yield* pollQueue.pipe(Effect.forever, Effect.forkScoped)
})
```

## Fiber API (`effect/Fiber`)

- `Fiber.join(fiber)` — wait; succeed/fail like the fiber.
- `Fiber.await(fiber)` — wait; returns `Exit` (never fails).
- `Fiber.interrupt(fiber)` — interrupt and wait for completion.
  `Fiber.interruptAs(fiber, id)`.
- `Fiber.joinAll`, `Fiber.awaitAll`, `Fiber.interruptAll`, `Fiber.interruptAllAs`.
- `Fiber.runIn(fiber, scope)` — interrupt a manually-started fiber when the scope closes.
- `Fiber.getCurrent()`, `Effect.fiber`, `Effect.fiberId` (a `number` in v4;
  `FiberId` module removed).
- Fiber object members: `id`, `pollUnsafe()`, `interruptUnsafe()`,
  `addObserver(cb)`, `getRef(reference)`.

`Fiber` is **not** an `Effect` subtype in v4: use `Fiber.join(fiber)`, not
`yield* fiber` (`references/migration/yieldable.md`).

## Racing

| API | Winner | Losers |
|---|---|---|
| `Effect.race(a, b)` | first **success** (fails only if both fail) | interrupted |
| `Effect.raceFirst(a, b)` | first **completion** (success or failure) | interrupted |
| `Effect.raceAll(effects)` | first success | interrupted |
| `Effect.raceAllFirst(effects)` | first completion | interrupted |

All accept `{ onWinner?: ({ fiber, index, parentFiber }) => void }`.
4.0.0 fix: losers are interrupted even if the race settles (or is interrupted)
while other effects are still starting — no orphaned losers.
v3 `Effect.raceWith` → `raceFirst` or fork both and inspect `Fiber.await`.

Timeouts: `Effect.timeout(d)` (fails with `Cause.TimeoutError`),
`Effect.timeoutOption(d)`, `Effect.timeoutOrElse({ duration, orElse })` (v3
`timeoutFail` / `timeoutTo`). The source is fully interrupted before the
fallback runs.

## Fiber collections: FiberHandle, FiberSet, FiberMap

Scoped containers that own fibers and interrupt them when the scope closes.

```ts
import { Effect, FiberHandle, FiberMap, FiberSet } from "effect"

const program = Effect.scoped(Effect.gen(function*() {
  // At most one fiber; run() interrupts the previous one
  const handle = yield* FiberHandle.make()
  yield* FiberHandle.run(handle, refreshLoop)
  yield* FiberHandle.run(handle, refreshLoop, { onlyIfMissing: true }) // keep existing

  // Many fibers, removed automatically when they finish
  const set = yield* FiberSet.make<void, WorkError>()
  yield* FiberSet.run(set, job(1))
  yield* FiberSet.run(set, job(2))
  yield* FiberSet.awaitEmpty(set) // wait until all current fibers finish
  // FiberSet.join(set): fails with the first fiber failure; it does NOT
  // complete when fibers succeed (use it to "run until something fails")

  // Keyed: run() with an existing key interrupts the old fiber
  const map = yield* FiberMap.make<string>()
  yield* FiberMap.run(map, "user-1", syncUser("user-1"))
  yield* FiberMap.remove(map, "user-1")
  yield* FiberMap.awaitEmpty(map)
}))
```

- `run` options: `onlyIfMissing` (Handle/Map), `propagateInterruption`,
  `startImmediately`.
- `FiberSet.runtime(set)<R>()` / `FiberMap.runtime` / `FiberHandle.runtime`
  return a function that forks effects from non-Effect callbacks into the
  collection (`runtimePromise` for a Promise result). `FiberSet.makeRuntime<R>()`
  combines both.
- Also: `clear`, `size`, `awaitEmpty`, `FiberMap.get/has`, `FiberHandle.get`.

## Pitfalls

- v3 names: `Effect.fork`, `Effect.forkDaemon`, `Effect.forkAll`,
  `Effect.withConcurrency`, `concurrency: "inherit"`, `mode: "either"`.
- Assuming `partition` returns `[failures, successes]` (order flipped in 4.0.0).
- Forking without an owner strategy: `forkDetach` leaks unless you keep the
  fiber and interrupt it.
- `Effect.scoped` around `forkScoped` that should live longer: the fiber is
  interrupted when that scope closes.
- Expecting `Effect.runPromise` to keep Node alive while fibers wait on
  `Deferred`/`Queue` only — use `runMain` (see `runtime-execution.md`).
- Unbounded parallelism against external systems; pick a number.
