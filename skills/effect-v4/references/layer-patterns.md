# Layer Patterns

Use this guide when constructing, composing, and providing layers. Service
definitions are covered in `references/dependency-management.md`.

Official examples:
- `references/examples/01_effect/03_services/20_layer-composition.ts` (provide vs provideMerge)
- `references/examples/01_effect/03_services/20_layer-unwrap.ts` (`Layer.unwrap`)
- `references/examples/01_effect/05_resources/10_acquire-release.ts` (scoped resources in layers)
- `references/examples/01_effect/05_resources/20_layer-side-effects.ts` (`Layer.effectDiscard`)
- `references/examples/01_effect/05_resources/30_layer-map.ts` (`LayerMap.Service`)
- `references/examples/01_effect/06_running/20_layer-launch.ts` (`Layer.launch`)

`Layer<ROut, E, RIn>`: builds `ROut`, may fail with `E`, needs `RIn`.

## Constructors

| Need | v4 | v3 equivalent |
|---|---|---|
| Pure value | `Layer.succeed(Key, impl)` / `Layer.succeed(Key)(impl)` | same |
| Lazy pure value | `Layer.sync(Key, () => impl)` | same |
| Effectful (incl. scoped resources) | `Layer.effect(Key, effect)` — `Scope` is excluded from `RIn` | `Layer.effect` **and** `Layer.scoped` |
| Several services from one effect | `Layer.effectContext(effect)` | `Layer.scopedContext` |
| Run an effect, provide nothing (background tasks, init) | `Layer.effectDiscard(effect)` | `Layer.scopedDiscard` |
| Choose a layer at build time | `Layer.unwrap(effectOfLayer)` | `Layer.unwrapEffect` / `unwrapScoped` |
| Lazily construct | `Layer.suspend(() => layer)` | same |
| From existing Context | `Layer.succeedContext(ctx)` / `Layer.syncContext` | `Layer.succeedContext` |
| Partial test double | `Layer.mock(Key, partialImpl)` | `Layer.mock` |
| No-op | `Layer.empty` | same |

`Layer.scoped*` no longer exists: any `acquireRelease` inside `Layer.effect` is
tied to the layer's lifetime automatically.

```ts
import { Config, Context, Effect, Layer } from "effect"

class Pool extends Context.Service<Pool, { readonly query: (sql: string) => Effect.Effect<unknown> }>()(
  "myapp/Pool"
) {
  static readonly layer = Layer.effect(
    Pool,
    Effect.gen(function*() {
      const url = yield* Config.String("DATABASE_URL")
      const conn = yield* Effect.acquireRelease(
        Effect.sync(() => ({ url, close: () => {} })),
        (conn) => Effect.sync(() => conn.close())
      ) // released when the layer's scope closes
      return Pool.of({ query: (_sql) => Effect.succeed(conn.url) })
    })
  )
}
```

### `Layer.unwrap` — decide at build time

```ts
static readonly layer = Layer.unwrap(
  Effect.gen(function*() {
    const inMemory = yield* Config.Boolean("STORE_IN_MEMORY").pipe(Config.withDefault(false))
    if (inMemory) return MessageStore.layerInMemory
    return MessageStore.layerRemote(yield* Config.URL("STORE_URL"))
  })
)
```

### `Layer.effectDiscard` — background work as a layer

```ts
const Heartbeat = Layer.effectDiscard(
  Effect.gen(function*() {
    yield* Effect.logInfo("tick").pipe(
      Effect.repeat(Schedule.spaced("5 seconds")),
      Effect.forkScoped // interrupted when the layer scope closes
    )
  })
)
```

## Composition

```ts
// Expose only UserRepository; SqlClient is an internal dependency
static readonly layer = this.layerNoDeps.pipe(Layer.provide(SqlClientLayer))

// Expose UserRepository AND SqlClient
static readonly layerWithSql = this.layerNoDeps.pipe(Layer.provideMerge(SqlClientLayer))

// Independent layers side by side (built concurrently)
const AppLayer = Layer.mergeAll(UserRepository.layer, Mailer.layer, Heartbeat)

// provide / provideMerge / merge also accept arrays
const Live = MyService.layerNoDeps.pipe(Layer.provide([Database.layer, Logger.layer]))
```

- `Layer.provide(dep)`: `RIn` of self is satisfied by `dep`; output = self only.
- `Layer.provideMerge(dep)`: same, output = self + dep.
- `Layer.merge(a, b)` / `Layer.mergeAll(...)`: union of outputs and inputs.
- `Layer.flatMap(self, (ctx) => layer)`: build a layer from another's output.
- Prefer one composed `AppLayer` provided once at the edge.

## Error handling and observability

- `Layer.catch(f)`, `Layer.catchTag("Tag", f)` (also accepts an array of tags),
  `Layer.catchCause(f)`, `Layer.orDie` (v3 `catchAll`/`catchAllCause` renamed).
- Retrying: retry the acquisition effect before `Layer.effect`
  (`Effect.retry(acquire, schedule)`); `Layer.retry` is gone.
- `Layer.tap`, `Layer.tapError`, `Layer.tapCause` observe construction.
- `Layer.withSpan("name")` wraps construction in a span; `Layer.updateService(Key, f)`
  adjusts a service/reference while building (replaces `Layer.locally*`).
- Type assertions: `Layer.satisfiesSuccessType<T>()`, `satisfiesErrorType`,
  `satisfiesServicesType`.

See `references/migration/api-map/effect-Layer.md` for every v3 → v4 mapping.

## Memoization (verified against 4.0.1 runtime)

- Within one build, a layer **value** used in several places is built once
  (memoized by reference). Two separately constructed layer values (e.g. calling
  `makeLayer()` twice) are different layers. Store layers in constants /
  `static readonly` fields.
- Nested provides share: the memo map is carried in the context
  (`Layer.CurrentMemoMap`) and forked, so an inner `Effect.provide(L)` reuses
  `L` if an **outer** provide already built it:

```ts
program.pipe(Effect.provide(L), Effect.provide(L)) // L built ONCE (v3: twice)
```

- Sibling provides do **not** share: `Effect.all([a.pipe(Effect.provide(L)),
  b.pipe(Effect.provide(L))])` builds `L` twice. Layers first built inside a
  nested provide are private to it (forked map, not written back).
- Opt out: `Layer.fresh(L)` (fresh memo map for that layer) or
  `Effect.provide(L, { local: true })` (whole subtree built from scratch).
- `ManagedRuntime.make(layer, { memoMap })` with a shared
  `Layer.makeMemoMapUnsafe()` shares builds across runtimes.

Bottom line: compose once and provide once; treat cross-provide sharing as a
safety net. See `references/migration/layer-memoization.md`.

## LayerMap — keyed, dynamically built layers

```ts
import { LayerMap } from "effect"

export class PoolMap extends LayerMap.Service<PoolMap>()("app/PoolMap", {
  lookup: (tenantId: string) => DatabasePool.layer(tenantId),
  // or a static set: layers: { acme: ..., globex: ... }, with preload?: boolean
  idleTimeToLive: "1 minute" // released after being unused this long
  // dependencies: [SomeDep.layer] -> provided to PoolMap.layer
}) {}

const program = Effect.gen(function*() {
  yield* queryCurrentTenant.pipe(Effect.provide(PoolMap.get("acme")))
  yield* PoolMap.invalidate("acme") // rebuild on next access
}).pipe(Effect.provide(PoolMap.layer))
```

Static members: `layer`, `layerNoDeps`, `get(key)` (a Layer),
`contextEffect(key)`, `contextEffectOption(key)` (only if already cached),
`invalidate(key)`. Options: `lookup` | `layers`, `dependencies`,
`idleTimeToLive`, `preloadKeys` (with `lookup`) / `preload` (with `layers`).
Preloading is skipped for keys whose idle TTL is zero (the default), so set a
non-zero `idleTimeToLive` if you want eager validation. Non-service form:
`LayerMap.make(lookup, options)` / `LayerMap.fromRecord(layers, options)`
(require `Scope`).

## LayerRef — one shared, refreshable layer (new in v4)

`LayerRef` is the unkeyed counterpart of `LayerMap`: builds a layer lazily,
shares the resulting context, and can invalidate/rebuild it.

```ts
import { LayerRef } from "effect"

class DatabaseRef extends LayerRef.Service<DatabaseRef>()("app/DatabaseRef", {
  layer: Database.layer,
  idleTimeToLive: "5 minutes",
  preload: true, // build during DatabaseRef.layer construction
  invalidationSchedule: Schedule.spaced("1 hour") // optional periodic rebuild
}) {}

const program = useDatabase.pipe(
  Effect.provide(DatabaseRef.get), // Layer<Database, E, DatabaseRef>
  Effect.provide(DatabaseRef.layer)
)
// DatabaseRef.invalidate  -> next use rebuilds
// DatabaseRef.refresh     -> invalidate and reacquire now
```

`LayerRef.make(layer, options)` returns a `LayerRef` value (`get`,
`contextEffect`, `invalidate`, `refresh`, `rcRef`) and requires `Scope`.
Invalidation does not revoke contexts already borrowed by open scopes.

## Running a layer as the app

`Layer.launch(layer)` builds the layer and never returns (`Effect<never, E, RIn>`);
interrupting it closes the layer scope and runs finalizers. Combine with
`NodeRuntime.runMain` — see `runtime-execution.md`.

```ts
const main = Layer.launch(Layer.mergeAll(HttpServerLive, Heartbeat))
NodeRuntime.runMain(main)
```

Manual building: `Layer.build(layer)` (needs `Scope`),
`Layer.buildWithScope(layer, scope)`, `Layer.buildWithMemoMap(layer, memoMap, scope)`
all return `Context.Context<ROut>`.

## Pitfalls

- Using `Layer.scoped`, `Layer.scopedDiscard`, `Layer.unwrapEffect`,
  `Layer.catchAll`, `Layer.retry`, `Layer.toRuntime` — removed/renamed.
- Creating layers inside functions called per-request (defeats memoization).
- Relying on sibling `Effect.provide` calls to share a resource.
- Forgetting `Layer.provide` for `layerNoDeps` and getting deps in `R` at the edge.
