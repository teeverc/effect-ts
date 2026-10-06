# Changelog digest: 4.0.0-beta.92 → 4.0.1 (stable)

Curated from `packages/*/CHANGELOG.md` in https://github.com/Effect-TS/effect
(sections `4.0.0-beta.93` through `4.0.1`). The previous skill baseline was
`4.0.0-beta.92` (June 30, 2026). Only items an agent must act on, or new APIs
worth reaching for, are listed. Every "new" name below was checked against the
4.0.1 source. For v3 → v4 migration, start at `references/migration.md`.

## (a) Stable release summary (4.0.0 / 4.0.1)

- **4.0.0 is the first stable v4 release**; 4.0.1 is a patch release (bug fixes
  only). Effect 4.x is an LTS line: at least three years of support.
- **One version for everything.** `effect` and every `@effect/*` package are
  released together; install the same version (e.g. all `4.0.1`).
- **Merged into `effect`:** `@effect/platform`, `rpc`, `cluster`, `cli`, `ai`,
  `sql`, `workflow`, `experimental`. Separate packages remain for platforms
  (`@effect/platform-node|bun|deno|browser|node-shared`), SQL drivers
  (`@effect/sql-pg|pglite|mysql2|mssql|libsql|clickhouse|d1|sqlite-*`), AI providers
  (`@effect/ai-anthropic|openai|openai-compat|openrouter|typesafe|cloudflare`),
  `@effect/atom-react|solid|vue`, `@effect/opentelemetry`, `@effect/vitest`,
  `@effect/openapi-generator`, `@effect/doctest`, `@effect/docgen`.
- **Import paths (rc.118):** `effect/unstable/*` was removed with **no compat
  exports**. Use `effect/<area>` barrels or direct modules:
  `effect/http`, `effect/http-api`, `effect/rpc`, `effect/cluster`,
  `effect/workflow`, `effect/ai`, `effect/cli`, `effect/sql`, `effect/schema`,
  `effect/socket`, `effect/process`, `effect/observability`,
  `effect/persistence`, `effect/reactivity`, `effect/encoding`, `effect/net`,
  `effect/eventlog`, `effect/workers`, `effect/devtools`, `effect/testing`.
  `import { HttpClient } from "effect/http"` or `effect/http/HttpClient`.
  The HttpApi entry point is `effect/http-api` (was `effect/httpapi`).
- **Stability tags:** `@stability unstable` = breaking changes allowed in minor
  releases; `@stability experimental` = breaking changes allowed in patches;
  untagged = strict semver. Moving modules out of `unstable/` did **not**
  stabilize them. APIs exposing third-party deps (NodeRedis, Undici, `ws`
  options, driver clients, AI provider schemas, `@effect/opentelemetry`) are
  tagged unstable.
- **Requirements:** TypeScript >= 5.9 with `strict` (TS 7 recommended);
  `@effect/vitest` and `@effect/doctest` need Vitest 5 (`>=5 <6`);
  `@effect/atom-react` needs React 19; `@effect/platform-deno` needs Deno >= 2.8.3;
  `@effect/sql-sqlite-node` needs Node >= 22.16. `effect` has no runtime deps.
- **Removed in v4:** `Micro` (no replacement module; use `Effect`), `STM`/`T*`
  modules (use `Effect.tx` + `TxRef`, `TxQueue`, `TxHashMap`, ...), `Either`
  (use `Result`), `FiberRef` (use `Context.Reference`), `Runtime<R>`.

## (b) Breaking / behavior changes since beta.92

### Core

| Old (beta) | New (4.0.x) | Since |
|---|---|---|
| `Array/Chunk/Effect/Record.partition`, `separate`, `Option.partitionMap` return `[failures, successes]` | return `[successes, failures]` — swap destructuring | 4.0.0 |
| `Queue.State.takers` entries called as functions | `entry.resume(...)` (`Queue.Taker`) | 4.0.0 |
| `Scope.close(scope)` on any `Scope` | requires `Scope.Closeable` (from `Scope.make` / `Scope.fork`); same for `closeUnsafe` | rc.118 |
| `Effect.withConcurrency`, `References.CurrentConcurrency`, `concurrency: "inherit"` | removed; pass a number or `"unbounded"` explicitly | beta.102 |
| `Effect.orElseSucceed(() => x)` | fallback now receives the error: `(e) => x` | rc.116 |
| `Context.mutate`, `Context.getReferenceUnsafe` | removed; contexts use overlays, `Context.get` resolves reference defaults | beta.103 |
| `Effect.schedule` / `scheduleFrom` error type | now includes schedule errors | beta.104 |
| `Queue.shutdown` always `true` | returns `false` if already shut down/completed; new `Queue.shutdownUnsafe` | rc.117 |
| `SynchronizedRef` usable with `Ref.*` | no longer a `Ref` subtype; use `SynchronizedRef.*` | rc.113 |
| `Graph.Proto` | `Graph.Graph<N, E, Graph.Kind>` | rc.113 |
| Custom `Clock` with only wall time | interface adds `monotonicTimeNanos` / `monotonicTimeNanosUnsafe` (wall clock vs monotonic split) | beta.103 |
| `ByteSize.Input` accepted any string | only canonical literals; parse others with `ByteSize.fromString` | rc.116 |
| `effect/Encoding` | `effect/encoding/Base64`, `Base64Url`, `Hex`, `EncodingError` | rc.113 |
| `Random.Random` / `Metric.MetricRegistry` phantom types | exported service interfaces (annotate custom impls directly) | rc.113 |
| Interrupted `Effect.cached*`, `Cache`, `ScopedCache`, `RcRef`, `RcMap` lookups | interruption = abandonment; never cached; shared computation interrupted only when all callers leave | 4.0.1 |
| `Effect.race*` losers kept starting | losers interrupted when race settles | 4.0.0 |

### Config (rc.113 / beta.103 / beta.95)

- Constructors are PascalCase: `Config.String`, `Number`, `Finite`, `Int`,
  `Boolean`, `Literal`, `Literals`, `Array`, `Record`, `Duration`, `ByteSize`,
  `Port`, `LogLevel`, `Redacted`, `URL`, `Date`, `NonEmptyString`.
  Lowercase `Config.string(...)` etc. no longer exist.
- `Config.mapOrFail` → `Config.mapEffect`. Added `Config.flatMap`.
- `Config.schema` rejects opaque schemas (`Schema.Any/Unknown/Json`); use a
  concrete shape or `Schema.fromJsonString(Schema.Json)`. Partially supplied
  `Config.all` groups are rejected.
- Built-in `ConfigProvider`s treat **empty strings as missing** (beta.95).
  `ConfigProvider` lookups signal absence with `undefined`.

### Schema

| Old | New | Since |
|---|---|---|
| `Schema.ErrorClass` / `Schema.TaggedErrorClass` | `Schema.Error` / `Schema.TaggedError` | beta.104 |
| `Schema.Error` (JS `Error` instance schema) | `Schema.ErrorInstance` | beta.104 |
| standalone `effect/SchemaError` module | `Schema.SchemaError`, `Schema.isSchemaError` | rc.108 |
| `Schema.isLengthBetween`, `isCodePointsBetween`, `isSizeBetween`, `isPropertiesLengthBetween`, `isStartsWith`, `isEndsWith`, `isIncludes` | `Schema.isBetweenLength`, `isBetweenCodePoints`, `isBetweenSize`, `isBetweenProperties`, `isStartingWith`, `isEndingWith`, `isIncluding` | rc.118 |
| `SchemaGetter.transformOrFail` / `SchemaTransformation.transformOrFail` | `transformEffect` | rc.113 |
| `SchemaTransformation.make` | `SchemaTransformation.makeTransformation` | rc.116 |
| `first.compose(second)` on transformations | `SchemaTransformation.composeTransformation(first, second)` | rc.116 |
| `new SchemaGetter.Getter(...)`, `onSome`, `onNone`, getter methods | `SchemaGetter.transformOptionalEffect` / `transformEffect`; `SchemaGetter.map/compose/run` | rc.116 |
| `Schema.toArbitrary`, `toArbitraryLazy`, `effect/testing/FastCheck` | native `Arbitrary.schema` from `effect/Arbitrary` (no fast-check) | rc.113 |
| `Schema.Annotations.ToArbitrary.Constraint` | `...ToArbitrary.FilterConstraint` | rc.114 |
| `Schema.DateValid`, `isDateValid` | `Schema.Date` itself rejects invalid dates | beta.102 |
| `Schema.UnknownFromJsonString` | `Schema.fromJsonString(Schema.Unknown)` | beta.103 |
| `SchemaIssue.actual`, `getActual`, `Schema.redact` | removed; opt in with parse option `reportInput` | beta.103/105 |
| `Schema.Record(..., { keyValueCombiner })` | removed | beta.103 |
| `onExcessProperty: "preserve"`, `propertyOrder` parse options | removed (use `Record`/`StructWithRest`) | rc.113 |
| `parseOptions` annotations | no effect; pass options to the parser call | rc.113 |
| `SchemaAST.Union.mode` | `SchemaAST.Union.options?.mode` (public `Schema.Union(members, { mode })` unchanged) | rc.113 |
| `Class.make(existingInstance)` re-constructed | returns the instance unchanged | rc.113 |
| Cookie/Headers/UrlParams schemas in `effect/http` | `Schema.Cookie`, `Schema.Cookies`, `Schema.Headers`, `Schema.UrlParams` | rc.113 |
| Built-in revivers in `Schema` | moved to `SchemaRepresentation` | rc.113 |
| `Schema.brand` with multiple ids / runtime brand annotations | single identifier, type-only; repeat `brand`/`fromBrand` to compose | 4.0.0 |
| `TestSchema ... verifyLosslessTransformation` | `verifyRoundTrip` (+ `succeedEffect`, `failEffect`, `verifyRoundTripEffect`) | 4.0.0 |

### Streams / Channel / Schedule

- `Stream.partition` returns `[passes, fails]` and takes `{ capacity }`.
- `Stream.mapBoth({ onElement, onError })`; `Stream.scan` / `scanEffect` take a
  **lazy** initial state (`() => s`); `Stream.catchTags` rejects unknown keys.
- `Channel.runDone` removed → `Channel.runDrain`.
- `Schedule.andThen` / `andThenResult` → `Schedule.concat` / `concatResult` (beta.104).
- `Schedule.both*` → `Schedule.max`; `Schedule.either*` → `Schedule.min` (beta.95).
- Removed: `Schedule.elapsed`, `tapInput`, `tapOutput` (use `Schedule.tap`),
  `collectInputs`, `collectOutputs`, `collectWhile`, `delays`, `reduce`,
  `unfold`, `satisfies*Type`. `addDelay` / `modifyDelay` receive full metadata.
- `Stream.aggregateWithin` / `groupedWithin` pause schedules while idle (4.0.1);
  `Stream.scan` emits initial state for empty streams (4.0.1).

### HTTP / HttpApi

- `effect/httpapi` → `effect/http-api` (also service keys/TypeIds).
- `UrlParams.makeUrl` → `Url.make` (fails with `Url.UrlError`) (beta.93).
- `HttpApi.ParseOptions` (API/group/endpoint) and per-slot parse options
  (rc.116/118). `HttpRouter` builds a fresh router per entrypoint (rc.118).
- Server WebSockets close 1000/1001/1011 by outcome (4.0.0).
- `HttpClient.catch` data-first overload constrained; `HttpBody` content
  lengths are bigint-exact; `FileSystem.File.seek` returns `bigint` (Node/Deno).
- `mime` dependency removed → `effect/http/Mime`.

### RPC / Cluster / Workflow

- MessagePack serialization and `msgpackr` removed; RPC serialization is
  schema-aware; cluster transports default to `SchemaBinary` (rc.112/113).
- RPC ids are `string | number`; `RpcMessage.ExitEncoded` interrupt `fiberId`
  may be `null` (4.0.0).
- Custom `MessageStorage.clearReplies` must honor the optional expected reply id
  (4.0.0). `EventLogEncryption` wire shape changed (upgrade clients and servers
  together, beta.106).
- `Model.Generated` → `Model.GeneratedByDb` (`effect/schema/Model`).

### AI

- `LanguageModel`, `EmbeddingModel`, `Chat`, `Reactivity` are branded
  interfaces: use the same-name type, not `.Service` / `["Service"]`; custom
  impls need `[TypeId]: TypeId` (rc.113).
- `Toolkit.handle` requires handler services on the outer Effect, and its
  return-mode stream exposes `AiError` (rc.118).
- Tool parameter validation failures route through `failureMode`;
  `ToolParameterValidationError.toolParams` removed (rc.113).
- `@effect/ai-amazon-bedrock` and `@effect/ai-google` do not exist in v4.

### CLI (rc.113)

- Constructors PascalCase in `Primitive`, `Param`, `Flag`, `Argument`:
  `integer` → `Int`, `float` → `Finite`, `none` → `Never`, `choice` → `Literals`
  (`Primitive.Choice`), others capitalized (`Flag.String`, `Flag.Boolean`, ...).
- `Prompt.text` → `Prompt.String`, `integer` → `Int`, `float` → `Number`.
  `GlobalFlag.action/setting` → `Action/Setting`.
- `Command.withHidden` → `Command.unlisted` (beta.104). Boolean flags are
  required when omitted unless given optional/default/config fallback (rc.110).

### SQL

- `@effect/sql-pg` has its own PostgreSQL client (no `pg` dep); `timestamp(tz)`
  decodes to `Date`; default idle timeout 60s.
- `@effect/sql-sqlite-node` uses `node:sqlite` (no `better-sqlite3`).
- `@effect/sql-mssql` encrypts and validates certificates by default.
- `PgClient.listen` returns a scoped dequeue. `SqlError` has structured reasons
  (e.g. `UniqueViolation`).

### Observability

- `@effect/opentelemetry` modules renamed: `Tracer` → `OtelTracer`,
  `Logger` → `OtelLogger`, `Metrics` → `OtelMetrics` (plus `NodeSdk`,
  `WebSdk`, `Resource`). Prefer `effect/observability` `Otlp*` for new apps.
- OTLP signal layers expose a `Flusher` service; custom
  `OtlpExporter.make` requires it. Explicit OTLP resource config beats env.
- Text logger levels are uppercase; `Logger.consolePretty` lost `stderr`.

### Testing

- `@effect/vitest`: Vitest 5 only; property tests use native `Arbitrary`
  (raw fast-check arbitraries and the `fastCheck` option are gone);
  `it.effect.each` passes test context as 2nd arg; `layer(..., { concurrent })`.
- `Arbitrary.configureGlobal` sets defaults, including the `@effect/vitest` run count.

## (c) Notable new APIs (verified in 4.0.1)

- Effect: `Effect.setContext`, `Effect.updateServiceScoped`, `Effect.head`,
  `Effect.reduce`, `Effect.cachedWithTTL` with per-`Exit` TTL,
  `Effectable.Mixin`.
- Concurrency/data: `Semaphore.takeIfAvailable`, `Queue.flush`,
  `Queue.shutdownUnsafe`, `Pool.use`, `RcMap.getOption`,
  `LayerMap.contextEffectOption`, `LayerRef`, `TxChunk`, `Record.assignProperty`,
  `Match.fn`, `Schedule.once`, `Cron.format`, `DateTime.toEpochSeconds` /
  `fromEpochSeconds`, `ConfigProvider.fromEnvRecord`, `Crypto` service
  (`randomUUIDv4`, `randomUUIDv7`, `randomULID`).
- Stream: `Stream.catchDefect`, `tapDefect`, `tapErrorTag`, `unwrapReason`,
  `as`; `Channel.catchDefect`; Web Stream interop for `Channel`/`Sink`.
- Schema: `Schema.Decoder` / `Schema.Encoder`, `Schema.Natural`,
  `Schema.JsonObject`, `Schema.DateFromMillis`, `Schema.isMinCodePoints` /
  `isMaxCodePoints`, `.matchOrElse` on `Schema.TaggedUnion` / `toTaggedUnion` schemas, `SchemaBinary`
  (`effect/encoding/SchemaBinary`), `StandardSchema` module, experimental
  JIT/AOT compilers under `effect/schema` (`SchemaJITCompiler`,
  `SchemaAOTCompiler`), `SchemaGetter.forbiddenEncoding`.
- HTTP: HTTP `QUERY` method, `HttpStatus.fromLiteral`, response compression,
  `HttpClient.withRateLimiter` 429 retries, `HttpClientRequest.updateHeaders` /
  `removeHeader`, `HttpClientResponse.url`, `HttpApiBuilder.handler`,
  `HttpApiSchema.WithHeaders` typed response headers, `HttpApiError.UnprocessableEntity`.
- Net/Socket: `effect/net` (`NetAddress`, `IpInterface`, `IpNetwork`),
  `socket.upgrade(...)` (STARTTLS), `NodeSocket.makeTls` / `layerTls`.
- Encoding: `effect/encoding` `Ini`, `Yaml`, `Toml` parsers.
- AI: `Decision` / `DecisionModel`, `Tool` `setNeedsApproval` method,
  `LanguageModel.supportsSystemMessagesInHistory` (4.0.1), MCP protocols
  2024-11-05 … 2026-07-28; providers `@effect/ai-typesafe`, `@effect/ai-cloudflare`.
- CLI: `Command.wizard` / `--wizard`, `CliConfig`, shell completions.
- Runtime: `ManagedRuntime` supports `await using` (`Symbol.asyncDispose`).
- Platform: `@effect/platform-deno` (full Deno support).

## (d) Stale guidance to reject

- `effect/unstable/...` or `effect/httpapi` imports → `effect/<area>/...`,
  `effect/http-api`.
- Separate `@effect/platform`, `@effect/rpc`, `@effect/cluster`, `@effect/cli`,
  `@effect/ai`, `@effect/sql`, `@effect/workflow`, `@effect/experimental` deps,
  or `@effect/*@0.x` versions not matching `effect`.
- `ServiceMap.*` (reverted name) → `Context.Service` / `Context.Reference`.
- `Context.Tag`, `Effect.Service`, `Effect.Tag` → `Context.Service`.
- `Effect.Yieldable`, `.asEffect()` on ordinary values → `Effectable.Class` /
  `Prototype` / `Mixin`, or module functions (`Ref.get`, `Fiber.join`).
- `Schema.makeUnsafe` / `SchemaParser.makeUnsafe` → `schema.make(input)` /
  `SchemaParser.make`; `Schema.Codec.ToAsserts` → `Schema.asserts(schema, input)`.
- `Schema.ErrorClass`, `Schema.TaggedErrorClass`, `Schema.DateValid`,
  `Schema.toArbitrary`, `effect/testing/FastCheck`, `SchemaGetter.transformOrFail`.
- `Micro` (removed in v4 — use `Effect`), `STM`/`TRef` (use `Effect.tx` / `TxRef`),
  `Either` (use `Result`), `FiberRef` (use `Context.Reference` / `References.*`).
- Lowercase `Config.string` / `Config.integer`, `Config.mapOrFail`.
- Lowercase CLI `Flag.string`, `Flag.integer`, `Flag.choice`, `Prompt.text`,
  `Command.withHidden`.
- `Schedule.andThen`, `Schedule.both`, `Schedule.either`, `Schedule.tapInput`.
- `Effect.withConcurrency`, `Channel.runDone`, `effect/Encoding`,
  `UrlParams.makeUrl`, `Random.nextUUIDv4` (use `Crypto`), `Model.Generated`.
- `@effect/opentelemetry/Tracer` (now `OtelTracer`), MessagePack RPC serialization.
- `[failures, successes] = Array.partition(...)` destructuring order.
