# Dependency Management (Context.Service, Context.Reference)

Use this guide when defining services, accessing them, and substituting them in
tests. For composing/wiring layers see `references/layer-patterns.md`.

Official examples: `references/examples/01_effect/03_services/` (`01_service.ts`,
`10_reference.ts`, `20_layer-composition.ts`, `20_layer-unwrap.ts`).

## Vocabulary

- **Service key** — `Context.Service<...>` value/class. It is itself an
  `Effect<Shape, never, Self>`, so `yield* Database` reads it.
- **Context** — `Context.Context<R>`, an immutable map of key → implementation
  (v3 `Context`, briefly `ServiceMap` in early betas, now `Context` again).
- **Reference** — `Context.Reference<A>`: a key with a default value; never
  shows up in `R`. Replaces v3 `FiberRef` for ambient config.
- **Layer** — `Layer<ROut, E, RIn>`: a recipe that builds services (see
  `layer-patterns.md`).

## Defining a service (class form — the default)

```ts
import { Context, Effect, Layer, Schema } from "effect"

export class DatabaseError extends Schema.TaggedError<DatabaseError>()("DatabaseError", {
  cause: Schema.Defect()
}) {}

// Context.Service<Self, Shape>()(id). The id should be globally unique
// (package + path); it is the runtime identity of the service.
export class Database extends Context.Service<Database, {
  query(sql: string): Effect.Effect<Array<unknown>, DatabaseError>
}>()("myapp/db/Database") {
  // Convention: attach the implementation as a static `layer`.
  static readonly layer = Layer.effect(
    Database,
    Effect.gen(function*() {
      const query = Effect.fn("Database.query")(function*(sql: string) {
        yield* Effect.log("Executing SQL query:", sql)
        return [{ id: 1, name: "Alice" }]
      })
      // `X.of` is an identity helper that type-checks the shape.
      return Database.of({ query })
    })
  )
}

// Need the service type? Use the `Service` member.
export type DatabaseService = Database["Service"]
```

Conventions (from upstream examples):

- Service methods are `Effect.fn("Service.method")(function*(...) {...})`.
- Name layers `layer` (deps provided), `layerNoDeps` (deps still in `RIn`),
  `layerTest` / `layerInMemory` for substitutes.
- Errors are `Schema.TaggedError` (or `Data.TaggedError`).
- Do not run effects in module scope or in constructors; do it in the layer.

### Other forms

```ts
import { Context, Effect, Layer } from "effect"

// Function form: no class, Identifier defaults to Shape.
interface Clock { readonly now: () => number }
const Clock = Context.Service<Clock>("myapp/Clock")

// Class form with a `make` option: the class gets a static `make`
// (an Effect or a function returning one). You still build the layer yourself.
class Greeter extends Context.Service<Greeter>()("myapp/Greeter", {
  make: Effect.gen(function*() {
    const clock = yield* Clock
    return { greet: (name: string) => `${clock.now()}: hi ${name}` }
  })
}) {
  static readonly layer = Layer.effect(this, this.make)
}
```

v4 does **not** generate a `Default` layer, `dependencies` wiring, or static
accessor proxies (v3 `Effect.Service` / `Effect.Tag`). Wire deps with
`Layer.provide` explicitly.

## Accessing services

```ts
import { Effect } from "effect"

// 1. yield* the key (preferred inside generators)
const a = Effect.gen(function*() {
  const db = yield* Database
  return yield* db.query("SELECT 1")
})

// 2. Key.use / Key.useSync for one-off access (common at framework edges)
const b = Database.use((db) => db.query("SELECT 1"))
const c = Database.useSync((db) => db.query) // pure projection

// 3. Optional access: does NOT add Database to R
const d = Effect.serviceOption(Database) // Effect<Option<DatabaseService>>
```

`Effect.service(Key)` also exists, but `yield* Key` is idiomatic.

Capture a service inside a layer (as above) rather than re-accessing it in every
method: methods then have `R = never`, which keeps service interfaces clean.

## Context.Reference (defaults, config, feature flags)

```ts
import { Context, Effect, Layer } from "effect"

export const FeatureFlag = Context.Reference<boolean>("myapp/FeatureFlag", {
  defaultValue: () => false
})

const program = Effect.gen(function*() {
  const enabled = yield* FeatureFlag // Effect<boolean, never, never>
  return enabled ? "new" : "old"
})

program.pipe(Effect.provideService(FeatureFlag, true)) // override for a region
Layer.succeed(FeatureFlag, true) // or override via a layer
```

- `defaultValue` is lazy and cached per key.
- Built-in references live in `References` (log level, tracer, scheduler...);
  see `concurrency-advanced.md`.
- `Effect.updateService(effect, Key, f)` transforms a service/reference for a
  region; `Effect.updateServiceScoped(Key, f)` until the current scope closes.

## Providing directly (without layers)

```ts
import { Context, Effect } from "effect"

// Single service value
program.pipe(Effect.provideService(Clock, { now: () => 0 }))

// Effectful acquisition of a single service
program.pipe(Effect.provideServiceEffect(Clock, Effect.sync(() => ({ now: Date.now }))))

// A whole Context
const ctx = Context.make(Clock, { now: () => 0 }).pipe(
  Context.add(Database, testDb)
)
program.pipe(Effect.provideContext(ctx))
// Effect.provide also accepts a Context, a Layer, or an array of Layers.
```

Capture the current context for running callbacks later:
`const ctx = yield* Effect.context<Database>()` then
`Effect.runForkWith(ctx)(effect)` / `Effect.runPromiseWith(ctx)(effect)`
(replaces v3 `Effect.runtime<R>()` + `Runtime.runFork`).

## Test substitutes

```ts
import { Effect, Layer } from "effect"

// Full fake
const DatabaseTest = Layer.succeed(Database, Database.of({
  query: (_sql) => Effect.succeed([])
}))

// Partial mock: omitted Effect/Stream-returning members die with
// "UnimplementedError" if called; non-effect members are required.
const DatabaseMock = Layer.mock(Database, {
  query: (_sql) => Effect.succeed([{ id: 1 }])
})

program.pipe(Effect.provide(DatabaseMock))
```

Stateful fakes: build them with `Layer.effect` + `Ref`, and expose the backing
`Ref` as its own service via `Layer.provideMerge` so tests can assert on it
(see `references/examples/09_testing/20_layer-tests.ts` and `testing.md`).

## v3 → v4

| v3 | v4 |
|---|---|
| `Context.Tag("Id")<Self, Shape>()` | `Context.Service<Self, Shape>()("Id")` |
| `Context.GenericTag<S>("Id")` | `Context.Service<S>("Id")` |
| `Effect.Tag` / `Effect.Service` (+ `Default`, `dependencies`, accessors) | `Context.Service` + explicit `static layer` + `Layer.provide`; use `X.use(...)` for accessors |
| `effect:` option of `Effect.Service` | `make:` option of `Context.Service` |
| `Context.Reference<Self>()("Id", { defaultValue })` / `FiberRef` | `Context.Reference<A>("Id", { defaultValue })` |
| `Effect.runtime<R>()` + `Runtime.runFork(rt)` | `Effect.context<R>()` + `Effect.runForkWith(ctx)` |
| `ServiceMap.*` (early v4 betas) | `Context.*` |

Details: `references/migration/services.md`,
`references/migration/api-map/effect-Context.md`,
`references/migration/api-map/effect-Effect.md` (`Effect.Service`, `Effect.Tag`).

## Pitfalls

- Reusing an id string for two different services: they share one slot.
- Putting `R` requirements on service *methods* instead of resolving them in
  the layer.
- Expecting `Database.Default` / `Database.query(...)` static accessors — they
  do not exist in v4.
- Importing `ServiceMap` or `FiberRef` — removed; use `Context` / `Context.Reference`.
