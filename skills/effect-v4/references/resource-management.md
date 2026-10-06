# Resource Management (Scope, acquireRelease, Pool, RcRef/RcMap)

Use this guide when acquiring and releasing resources deterministically.

Official examples: `references/examples/01_effect/05_resources/`
(`10_acquire-release.ts`, `20_layer-side-effects.ts`, `30_layer-map.ts`).

## Mental model

- A `Scope` collects finalizers; closing it runs them (LIFO for the default
  `"sequential"` strategy). Finalizers see the `Exit` the scope closed with.
- Anything that needs cleanup has `Scope` in its `R` until something closes a
  scope around it: `Effect.scoped`, a `Layer`, `Scope.use`, or a fiber/runtime.
- In application code, the most common owner of resources is a **layer**:
  `acquireRelease` inside `Layer.effect` lives as long as the layer.
- Scope finalizers run uninterruptibly (since 4.0.0 RCs), so cleanup cannot be
  abandoned midway.

## Core APIs

```ts
import { Effect, Scope } from "effect"

// acquire + release; release receives the resource and the scope's Exit
const file = Effect.acquireRelease(
  Effect.sync(() => openFile("/tmp/app.log")),
  (f, _exit) => Effect.sync(() => f.close())
) // Effect<File, never, Scope>

// Standard JS disposables (Symbol.dispose / Symbol.asyncDispose)
const handle = Effect.acquireDisposable(Effect.sync(() => makeDisposable()))

// Close the scope when the block ends
const program = Effect.scoped(
  Effect.gen(function*() {
    const f = yield* file
    yield* Effect.addFinalizer((exit) => Effect.log(`done: ${exit._tag}`))
    return f.path
  })
)

// Bracket without a Scope in R
const once = Effect.acquireUseRelease(
  Effect.sync(() => openFile("/tmp/x")),
  (f) => Effect.succeed(f.path),
  (f, _exit) => Effect.sync(() => f.close())
)
```

Other finalization helpers (not resource-owning): `Effect.ensuring(fin)`,
`Effect.onExit(f)`, `Effect.onError`, `Effect.onInterrupt((interruptors) => ...)`.

`acquireRelease` options: `{ interruptible?: boolean }` makes acquisition
interruptible (default: uninterruptible acquire).

## Working with Scope directly

```ts
import { Effect, Exit, Scope } from "effect"

const manual = Effect.gen(function*() {
  const scope = yield* Scope.make() // Scope.Closeable; "sequential" | "parallel"
  const f = yield* file.pipe(Scope.provide(scope)) // provide WITHOUT closing
  // ... use f across several steps ...
  yield* Scope.close(scope, Exit.void) // run finalizers
})

const child = Effect.gen(function*() {
  const parent = yield* Effect.scope // current Scope (requires Scope)
  const sub = yield* Scope.fork(parent) // closed with parent; can close early
  yield* Scope.addFinalizer(sub, Effect.log("sub closed"))
  yield* Scope.addFinalizerExit(sub, (exit) => Effect.log(exit._tag))
})

// Run with a closeable scope and close it with the effect's exit
const used = file.pipe(Scope.use(someCloseableScope))
```

- `Scope.close` / `Scope.closeUnsafe` require a `Scope.Closeable` (from
  `Scope.make` / `Scope.fork`), not a plain `Scope`.
- `Effect.scopedWith((scope) => ...)` creates a scope and hands it to you.
- v3 `Scope.extend` → `Scope.provide` (`references/migration/scope.md`).

## Pool

```ts
import { Effect, Pool } from "effect"

const program = Effect.scoped(
  Effect.gen(function*() {
    const pool = yield* Pool.make({
      acquire: Effect.acquireRelease(connect, (c) => c.close),
      size: 10 // fixed size; optional: concurrency (uses per item), targetUtilization
    })
    // Borrow for the duration of an effect (returned on any exit)
    const rows = yield* Pool.use(pool, (conn) => conn.query("SELECT 1"))
    // Or borrow for the current scope
    const conn = yield* Pool.get(pool)
    yield* Pool.invalidate(pool, conn) // discard a broken item
    return rows
  })
)
```

`Pool.makeWithTTL({ acquire, min, max, timeToLive, timeToLiveStrategy?: "creation" | "usage" })`
shrinks idle excess items. `Pool.reserve(pool, item)` keeps an item out of
shared circulation for the current scope. v3 `KeyedPool` → use `RcMap` of pools
or `LayerMap` (`references/migration/api-map/effect-KeyedPool.md`).

## RcRef and RcMap — shared, reference-counted resources

```ts
import { Effect, RcMap, RcRef } from "effect"

const program = Effect.scoped(
  Effect.gen(function*() {
    // One lazily-acquired resource, shared while borrowed
    const client = yield* RcRef.make({
      acquire: Effect.acquireRelease(makeClient, (c) => c.close),
      idleTimeToLive: "30 seconds" // keep alive after last borrower leaves
    })
    const c = yield* RcRef.get(client).pipe(Effect.scoped) // borrow per scope
    yield* RcRef.invalidate(client) // next get re-acquires

    // Keyed variant
    const sessions = yield* RcMap.make({
      lookup: (userId: string) => Effect.acquireRelease(openSession(userId), (s) => s.close),
      idleTimeToLive: "1 minute", // or (key) => duration
      capacity: 1000 // adds Cause.ExceededCapacityError to E
    })
    const s = yield* RcMap.get(sessions, "u1")
    const cached = yield* RcMap.getOption(sessions, "u2") // no acquire if absent
    yield* RcMap.invalidate(sessions, "u1")
    // also: RcMap.has, RcMap.keys, RcMap.touch
  })
)
```

Both `make`s require `Scope`: everything is released when the owning scope
closes. Use `LayerMap` / `LayerRef` (`layer-patterns.md`) when the resource is a
whole layer of services.

### 4.0.1: interruption is abandonment

In 4.0.1, `RcRef`, `RcMap`, `Cache`, `ScopedCache`, `Effect.cached`,
`Effect.cachedWithTTL`, `Effect.cachedInvalidateWithTTL` (and the cluster
`ResourceMap`) treat interruption of a caller as abandonment:

- Concurrent callers share one acquisition; it is interrupted only after
  **every** waiting caller has been interrupted.
- Interrupted results are never cached.
- A call arriving while an abandoned acquisition is still finalizing starts a
  fresh one.

So a single caller timing out (`Effect.timeout`) no longer kills an acquisition
other callers are waiting on, and you never get a cached "interrupted" entry.

## ScopedRef and Resource

```ts
import { Effect, Resource, Schedule, ScopedRef } from "effect"

// (inside Effect.gen, with a Scope available)
// ScopedRef: a mutable reference whose value owns a scope; set releases the old one
const ref = yield* ScopedRef.fromAcquire(acquireConn) // requires Scope
yield* ScopedRef.set(ref, acquireConn) // acquires new, then releases previous
const current = yield* ScopedRef.get(ref)

// Resource: a refreshable cached acquisition (failures are stored too)
const config = yield* Resource.manual(loadRemoteConfig) // requires Scope
const auto = yield* Resource.auto(loadRemoteConfig, Schedule.spaced("5 minutes"))
const value = yield* Resource.get(config)
yield* Resource.refresh(config)
```

If acquiring a replacement fails, `ScopedRef.set` keeps the current value alive.

## Fibers and scopes

Background work that uses scoped resources must not outlive them: fork it with
`Effect.forkScoped` (inside `Effect.scoped` or a layer) or into a
`FiberSet`/`FiberMap` (see `concurrency.md`).

## Pitfalls

- `Scope` left in `R` at the edge: add `Effect.scoped` or move the resource
  into a layer.
- Using a resource after its scope closed (returning it out of `Effect.scoped`).
- `Effect.scoped` around a short block that hands the resource to a background
  fiber: the resource is released when the block ends.
- Finalizers that fail silently: log/handle errors inside the release effect.
- v3 leftovers: `Scope.extend`, `Layer.scoped`, `Effect.acquireReleaseInterruptible`
  (use `acquireRelease(..., { interruptible: true })`).
