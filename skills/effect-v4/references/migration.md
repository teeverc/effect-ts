# Migrating from Effect v3 to v4 (4.0.x stable)

Entry point for upgrading a v3 codebase to Effect 4.0.x. Upstream source:
`MIGRATION.md` and `migration/*.md` in https://github.com/Effect-TS/effect
(copied into `references/migration/`). The generated per-symbol rename map is
split under `references/migration/api-map/`. For changes between the v4 betas
and stable, see `references/changelog-4.0.md`.

The programming model (`Effect`, `Layer`, `Schema`, `Stream`, ...) is the same.
What changed: package layout, versioning, import paths, and many names.

## 1. Packages, versions, imports

- **Single version.** Every Effect package is released together. Use the same
  version for `effect` and all `@effect/*` deps (e.g. `effect@4.0.1`,
  `@effect/platform-node@4.0.1`, `@effect/sql-pg@4.0.1`). v3-style
  `@effect/platform@0.x` / `@effect/sql@0.x` versions mean the code is pre-v4.
- **Merged into `effect` (remove these deps):** `@effect/platform`,
  `@effect/rpc`, `@effect/cluster`, `@effect/cli`, `@effect/ai`, `@effect/sql`,
  `@effect/workflow`, `@effect/experimental`. `@effect/typeclass` is gone
  (`Semigroup` → `effect/Combiner`, `Monoid` → `effect/Reducer`, see
  `api-map/typeclass.md`); `@effect/printer*` was removed with no replacement.
- **Still separate (bump to the same version):** `@effect/platform-node|bun|deno|browser`,
  `@effect/sql-*` drivers, `@effect/ai-anthropic|openai|openai-compat|openrouter|typesafe|cloudflare`,
  `@effect/atom-react|solid|vue`, `@effect/opentelemetry`, `@effect/vitest`.
  `@effect/ai-amazon-bedrock`, `@effect/ai-google`, `@effect/sql-drizzle` and
  `@effect/sql-kysely` have no v4 package.
- **Import paths:** core modules stay at `effect` / `effect/<Module>`; merged
  packages live under `effect/<area>` barrels:

| v3 import | v4 import |
|---|---|
| `@effect/platform/HttpClient`, `HttpRouter`, `HttpServer`, ... | `effect/http` (`effect/http/HttpClient`) |
| `@effect/platform/HttpApi*`, `OpenApi` | `effect/http-api` |
| `@effect/platform/FileSystem`, `Path`, `Terminal`, `Error` | `effect/FileSystem`, `effect/Path`, `effect/Terminal`, `effect/PlatformError` |
| `@effect/platform/Command`, `CommandExecutor` | `effect/process/ChildProcess`, `ChildProcessSpawner` |
| `@effect/platform/KeyValueStore`, `@effect/experimental/Persistence*` | `effect/persistence` |
| `@effect/platform/Socket`, `Worker` | `effect/socket`, `effect/workers` |
| `@effect/platform/Ndjson`, `MsgPack`, `@effect/experimental/Sse` | `effect/encoding/Ndjson`, `SchemaBinary`, `Sse` |
| `@effect/rpc/*`, `@effect/cluster/*`, `@effect/workflow/*` | `effect/rpc`, `effect/cluster`, `effect/workflow` |
| `@effect/cli/Options`, `Args`, `ValidationError` | `effect/cli/Flag`, `Argument`, `CliError` |
| `@effect/ai/*` | `effect/ai` |
| `@effect/sql/SqlClient`, `Model` | `effect/sql/SqlClient`, `effect/schema/Model` / `effect/sql/SqlModel` |
| `@effect/opentelemetry/Otlp*` | `effect/observability/Otlp*` |
| `@effect/platform-node/NodeContext` | `@effect/platform-node/NodeServices` |
| `effect/Either` | `effect/Result` |
| `effect/FiberRef` | `effect/References` (+ `Context.Reference`) |
| `effect/ParseResult` | `effect/SchemaIssue`, `effect/SchemaParser` |
| `effect/JSONSchema` | `effect/JsonSchema` |
| `effect/TRef`, `TQueue`, `TMap`, ... | `effect/TxRef`, `TxQueue`, `TxHashMap`, ... |
| `effect/TestClock` | `effect/testing/TestClock` |
| `effect/Micro` | removed — use `Effect` (the v4 runtime is small; ~6.3 KB min+gz) |

- `effect/unstable/*` paths (from v4 betas) **do not exist** in 4.0.x; drop the
  `unstable` segment (`effect/unstable/http` → `effect/http`).
- Stability: modules under `ai`, `cli`, `cluster`, `devtools`, `eventlog`,
  `http`, `http-api`, `observability`, `persistence`, `process`, `reactivity`,
  `rpc`, `schema`, `socket`, `sql`, `workflow`, `workers` are tagged
  `@stability unstable` (breaking changes in minors). See `references/versioning.md`.

Full import map and removed modules: `references/migration/api-map/README.md`
("Import Map", "No Counterpart Imports", "Removed Modules").

## 2. Quick reference: common renames

| v3 | v4 | Guide |
|---|---|---|
| `Context.Tag("Id")<S, Shape>()`, `Effect.Tag`, `Context.GenericTag` | `Context.Service<S, Shape>()("Id")` / `Context.Service<Shape>("Id")` | services |
| `Effect.Service` (+ `.Default`, `dependencies`) | `Context.Service` + explicit `static layer = Layer.effect(this, make)` | services |
| `FiberRef`, `Effect.locally` | `Context.Reference`, `References.*`, `Effect.provideService` | fiberref |
| `Either`, `Effect.either` | `Result`, `Effect.result` | api-map |
| `Effect.async` | `Effect.callback` | api-map |
| `Effect.zipRight` | `Effect.andThen` | api-map |
| `Effect.catchAll` / `catchAllCause` / `catchAllDefect` | `Effect.catch` / `catchCause` / `catchDefect` | error-handling |
| `Effect.catchSome` / `catchSomeCause` | `Effect.catchFilter` / `catchCauseFilter` | error-handling |
| `Effect.catchSomeDefect` | removed | error-handling |
| `Effect.fork` / `forkDaemon` | `Effect.forkChild` / `forkDetach` | forking |
| `Effect.forkAll`, `forkWithErrorHandler` | removed | forking |
| `Runtime<R>`, `Runtime.runFork(rt)` | `Context<R>`, `Effect.runForkWith(ctx)` | runtime |
| `Effect.gen(this, function*)` | `Effect.gen({ self: this }, function*)` | generators |
| yielding `Ref`, `Deferred`, `Fiber`, `Option` directly | `Ref.get`, `Deferred.await`, `Fiber.join`, `Effect.fromOption` | yieldable |
| `Cause.isFailure` / `isDie` / `isInterrupted` | `Cause.hasFails` / `hasDies` / `hasInterrupts` | cause |
| `Cause.failureOption` | `Cause.findErrorOption` | cause |
| `Cause.sequential` / `parallel` | `Cause.combine` | cause |
| `Cause.NoSuchElementException` / `TimeoutException` | `Cause.NoSuchElementError` / `TimeoutError` | cause |
| `Scope.extend` | `Scope.provide` | scope |
| `Equal.equivalence<T>()` | `Equal.asEquivalence<T>()` | equality |
| `Schema.decode` / `decodeUnknown` / `encode` | `Schema.decodeEffect` / `decodeUnknownEffect` / `encodeEffect` | schema |
| `Schema.decodeEither` | `Schema.decodeExit` (or `decodeResult`) | schema |
| `Schema.encodedSchema` / `typeSchema` / `asSchema` | `Schema.toEncoded` / `toType` / `revealCodec` | schema |
| `Schema.Union(a, b)`, `Tuple(a, b)`, `Literal("a", "b")` | `Schema.Union([a, b])`, `Tuple([a, b])`, `Literals(["a", "b"])` | schema |
| `Schema.Record({ key, value })` | `Schema.Record(key, value)` | schema |
| `Schema.compose`, `annotations`, `parseJson` | `Schema.decodeTo`, `annotate`, `fromJsonString` | schema |
| `Schema.Date` (from ISO string) | `Schema.DateFromString` (`Schema.Date` = old `DateFromSelf`) | schema |
| `Schema.TaggedError<S>()("Tag", {...})` | same name (`Schema.TaggedError`); `Data.TaggedError` also exists | schema |
| `Schema.arbitrary`, `effect/FastCheck` | `Arbitrary.schema` (native, `effect/Arbitrary`) | schema |
| `Config.string("X")`, `Config.integer` | `Config.String("X")`, `Config.Int` | api-map |

## 3. Ordered checklist

Work top to bottom; later steps depend on earlier ones.

1. **Dependencies.** Remove merged packages; set `effect` and every remaining
   `@effect/*` to one identical 4.0.x version. Require TypeScript >= 5.9
   (`strict: true`), Vitest 5 if using `@effect/vitest`.
2. **Imports.** Rewrite `@effect/platform|rpc|cluster|cli|ai|sql|workflow|experimental/*`
   and any `effect/unstable/*` to `effect/<area>` (table above, then
   `api-map/README.md`). Rename `NodeContext` → `NodeServices`.
3. **Services** → `references/migration/services.md`. `Context.Tag` /
   `Effect.Tag` / `Effect.Service` → `Context.Service`; build layers explicitly
   (`static layer = Layer.effect(this, this.make)`); name layers `layer`.
4. **FiberRef** → `references/migration/fiberref.md`. `FiberRef` →
   `Context.Reference` or `References.*`; `Effect.locally` → `Effect.provideService`.
5. **Error handling** → `references/migration/error-handling.md`. `catchAll*`
   → `catch*`, `catchSome*` → `catch*Filter`; new `catchReason(s)`,
   `catchEager`. `Either` → `Result`.
6. **Forking** → `references/migration/forking.md`. `fork` → `forkChild`,
   `forkDaemon` → `forkDetach`.
7. **Generators & subtyping** → `references/migration/generators.md`,
   `references/migration/yieldable.md`. `Ref`, `Deferred`, `Fiber`, `Option`,
   `Result` are no longer Effects; use module functions / `Effect.fromOption` /
   `Effect.fromResult`. Custom effect-like values: `Effectable.Class`,
   `Effectable.Prototype`, `Effectable.Mixin`.
8. **Runtime** → `references/migration/runtime.md`. `Runtime<R>` removed;
   capture `Effect.context<R>()` and use `Effect.runForkWith` / `run*With`.
   Use `ManagedRuntime` for framework integration.
9. **Cause** → `references/migration/cause.md`. Flat `cause.reasons`; replace
   tree walking; `isFailType` → `isFailReason`, `isDieType` → `isDieReason`.
10. **Scope** → `references/migration/scope.md`. `Scope.extend` → `Scope.provide`;
    only `Scope.Closeable` (from `Scope.make`/`fork`) can be closed.
11. **Layer memoization** → `references/migration/layer-memoization.md`. Layers
    are memoized across `Effect.provide` calls; isolate with `Layer.fresh` or
    `Effect.provide(layer, { local: true })`.
12. **Equality** → `references/migration/equality.md`. `Equal.equals` is
    structural for plain objects/arrays/Maps/Sets; opt out with `Equal.byReference`.
13. **Fiber keep-alive** → `references/migration/fiber-keep-alive.md` (read its
    4.0.1 note). Only `runMain` keeps the process alive; `Effect.runPromise` /
    `runFork` do not. Use `NodeRuntime.runMain` for long-lived programs.
14. **Schema** → `references/migration/schema.md` (long: summary table at top)
    and `references/schema.md`. Codec model, `decode*Effect`, array-arg
    `Union`/`Tuple`/`Literals`, `check(isX(...))` filters, native `Arbitrary`.
    Construct with `MySchema.make(input)` / `makeOption` / `makeEffect` or
    `SchemaParser.make`; assert with `Schema.asserts(schema, input)`.
15. **Module-specific APIs.** For each remaining compile error, look up the v3
    symbol in the API map (below). Areas with big redesigns: CLI (`Options` →
    `Flag`, PascalCase constructors), `Command` → `ChildProcess`, STM → `Effect.tx`
    + `Tx*`, Schedule (`concat`, `max`, `min`), Config (PascalCase constructors).
16. **Typecheck and test.** `tsc --noEmit`, then run tests; check for
    behavior changes listed in `references/changelog-4.0.md` (e.g. `partition`
    tuple order, empty env strings treated as missing).

## 4. Using the API map

`references/migration/api-map/` holds one file per v3 module
(`effect-Effect.md`, `effect-Schema.md`, `platform.md`, `cli.md`, `sql.md`, ...);
each line is `v3 symbol -> v4 replacement: note`.

```sh
# Find the replacement for a v3 symbol
grep -rn "Effect.catchAll\b" references/migration/api-map/
grep -rn "\`Schedule.andThen" references/migration/api-map/effect-Schedule.md
# Find where a v3 module moved
grep -n "@effect/platform/HttpApiBuilder" references/migration/api-map/README.md
# List everything removed with no counterpart
grep -n "-> \`none\`" references/migration/api-map/README.md
```

Index of files and the import map: `references/migration/api-map/README.md`.

## 5. Notes

- `ServiceMap` appeared in some v4 betas and was reverted: use `Context`.
- `Effect.Yieldable` and "call `.asEffect()` on any yieldable value" guidance from betas is stale.
  `asEffect()` only exists as the method you implement on `Effectable.Class`
  custom Effects (`references/migration/yieldable.md`).
- `Micro` was removed in v4; port `Micro.*` code to `Effect.*`
  (`references/migration/api-map/effect-Micro.md`).
- Prefer `Effect.fn("name")` for reusable effectful functions, `Effect.fnUntraced`
  in hot/library paths. See `references/llms.md` and `references/examples/`.
