# Migrating from Effect v3 to v4

Use this guide as the entry point when upgrading a v3 codebase to v4. It covers the full set of breaking changes with a quick-reference table and links to deep-dive guides for each topic.

## Package Changes

- All Effect ecosystem packages share a single version number and release together.
- Many packages are consolidated into `effect`; platform, SQL, AI, atom, and observability packages remain separate (`@effect/platform-*`, `@effect/sql-*`, `@effect/ai-*`, `@effect/atom-*`, `@effect/opentelemetry`, `@effect/vitest`).
- HTTP, HttpApi, AI, and other APIs are under `effect/unstable/*`. APIs under `unstable/` may receive breaking changes in minor releases.

## Quick-Reference: Common v3 → v4 Renames

| v3 | v4 |
|---|---|
| `Context` | `ServiceMap` |
| `Context.Tag("Id")()` | `ServiceMap.Service()("Id")` |
| `Effect.Service` | `ServiceMap.Service` |
| `Context.Reference` / `FiberRef` | `ServiceMap.Reference` / `References.*` |
| `Effect.either` | `Effect.result` |
| `Either` | `Result` |
| `Effect.catchAll` | `Effect.catch` |
| `Effect.catchAllCause` | `Effect.catchCause` |
| `Effect.catchSome` | `Effect.catchFilter` |
| `Effect.catchSomeCause` | `Effect.catchCauseFilter` |
| `Effect.catchSomeDefect` | Removed |
| `Effect.fork` | `Effect.forkChild` |
| `Effect.forkDaemon` | `Effect.forkDetach` |
| `Effect.forkAll` | Removed (use `Effect.forEach` or individual `forkChild`) |
| `Effect.locally` | `Effect.provideService` |
| `Runtime<R>` | Removed — use `ServiceMap<R>` |
| `Scope.extend` | `Scope.provide` |
| `Equal.equivalence<T>()` | `Equal.asEquivalence<T>()` |
| `Schema.decode` | `Schema.decodeEffect` |
| `Schema.encode` | `Schema.encodeEffect` |
| `Schema.encodedSchema` | `Schema.toEncoded` |
| `Schema.typeSchema` | `Schema.toType` |
| `Schema.Union(a, b)` | `Schema.Union([a, b])` |
| `Schema.Tuple(a, b)` | `Schema.Tuple([a, b])` |
| `Cause.isFailure` | `Cause.hasFails` |
| `Cause.isDie` | `Cause.hasDies` |
| `Cause.isInterrupted` | `Cause.hasInterrupts` |
| `Cause.failureOption` | `Cause.findErrorOption` |
| `Cause.sequential(l, r)` | `Cause.combine(l, r)` |
| `Cause.parallel(l, r)` | `Cause.combine(l, r)` |
| `Cause.NoSuchElementException` | `Cause.NoSuchElementError` |
| `Cause.TimeoutException` | `Cause.TimeoutError` |

## Migration Checklist

Work through these in order — later changes often depend on earlier ones.

### 1. Services and Context → `references/migration/services.md`
- Replace `Context.Tag` with `ServiceMap.Service`
- Replace `Context.Reference` with `ServiceMap.Reference`
- Update `FiberRef` usages to `References.*` (see `references/migration/fiberref.md`)

### 2. Error Handling → `references/migration/error-handling.md`
- Rename `catchAll` → `catch`, `catchAllCause` → `catchCause`, `catchAllDefect` → `catchDefect`
- Replace `catchSome` with `catchFilter`, `catchSomeCause` with `catchCauseFilter`
- Remove `catchSomeDefect` usages

### 3. Forking → `references/migration/forking.md`
- Rename `Effect.fork` → `Effect.forkChild`
- Rename `Effect.forkDaemon` → `Effect.forkDetach`
- Replace `forkAll` with `Effect.forEach` or individual `forkChild` calls

### 4. Runtime → `references/migration/runtime.md`
- Remove `Runtime<R>` type; use `ServiceMap<R>` in its place
- Run functions live directly on `Effect` — no `Runtime` module needed for execution

### 5. Generators and Yieldable → `references/migration/generators.md` + `references/migration/yieldable.md`
- Many v3 types no longer subtype `Effect`; call module helpers (`Ref.get`, `Deferred.await`, `Fiber.join`) inside `Effect.gen`
- Use `.asEffect()` to feed a Yieldable into Effect combinators

### 6. Cause → `references/migration/cause.md`
- `Cause` is now a flat `{ reasons: ReadonlyArray<Reason<E>> }` — remove tree-walking code
- Replace type-level guards (`isFailType`, `isDieType`) with reason-level guards (`isFailReason`, `isDieReason`)
- Replace `Cause.isFailure` → `Cause.hasFails`, `Cause.isDie` → `Cause.hasDies`

### 7. Scope → `references/migration/scope.md`
- Rename `Scope.extend` → `Scope.provide`

### 8. Layer Memoization → `references/migration/layer-memoization.md`
- Layers are now memoized across `Effect.provide` calls by default
- Use `Layer.fresh` or `Effect.provide(layer, { local: true })` for isolation

### 9. Equality → `references/migration/equality.md`
- `Equal.equals` now uses structural equality by default for plain objects/arrays/Maps/Sets
- Use `Equal.byReference` to opt back into reference equality
- Rename `Equal.equivalence<T>()` → `Equal.asEquivalence<T>()`

### 10. Schema → `references/schema.md`
- `Schema` is now a Codec; use `Schema.revealCodec`, `Schema.toType`, `Schema.toEncoded`
- Rename `decode*` / `encode*` → `decode*Effect` / `encode*Effect`
- `Schema.Union` / `Schema.Tuple` / `Schema.TemplateLiteral` now take arrays
- `validate*` APIs removed

### 11. Fiber Keep-Alive → `references/migration/fiber-keep-alive.md`
- Process keep-alive is now built into the runtime — no need to add `setInterval` hacks
- `runMain` from platform packages is still recommended for signal handling

## Unstable Modules

v4 ships with modules under `effect/unstable/*` (HTTP, HttpApi, AI, CLI, etc.). These may receive breaking changes in minor releases. Avoid them in production libraries; use them for prototyping or internal tooling.
