# Docs Index (topic → local reference → upstream doc)

Routing map for Effect 4.0.x tasks. Open one primary guide plus one adjacent
guide for cross-cutting concerns (testing, runtime, errors). Local files are
relative to `references/`.

## Local sources, in priority order

1. Topic guides: `references/*.md` (table below).
2. Official typechecked examples: `references/examples/` (index:
   `references/examples/index.md`) and the official LLM guide
   `references/llms.md` (copy of upstream `LLMS.md`).
3. Migration material: `references/migration.md`, `references/migration/*.md`,
   `references/migration/api-map/` (grep it for any v3 symbol).
4. Changes since the v4 betas: `references/changelog-4.0.md`.
5. The installed source: `node_modules/effect/dist/**/*.d.ts` JSDoc is
   authoritative for the version you have (`references/versioning.md`).

## Topic map

| Topic | Local guide(s) | Official example(s) |
|---|---|---|
| Effect basics, `Effect.gen`, `Effect.fn` | `core-usage.md`, `generators.md` | `examples/01_effect/01_basics/` |
| Promise/async/callback interop | `migration-async.md` | `examples/01_effect/01_basics/10_creating-effects.ts` |
| Services, `Context.Service`, `Context.Reference`, layers | `dependency-management.md`, `layer-patterns.md` | `examples/01_effect/03_services/` |
| Errors, `catchTag(s)`, reasons, defects | `error-management.md`, `error-tooling.md` | `examples/01_effect/04_errors/` |
| `Exit`, `Cause` | `exit-cause.md` | — |
| Resources, `Scope`, `acquireRelease`, `LayerMap` | `resource-management.md` | `examples/01_effect/05_resources/` |
| Running programs, `runMain`, `Layer.launch`, `ManagedRuntime` | `runtime-execution.md` | `examples/01_effect/06_running/`, `examples/04_integration/` |
| Fibers, `Semaphore`, `Latch`, `Deferred`, concurrency options | `concurrency.md`, `concurrency-advanced.md` | — |
| Queue, PubSub, Stream, STM (`Effect.tx`, `Tx*`) | `streams-queues-stm.md`, `sink.md` | `examples/01_effect/07_pubsub/`, `examples/03_stream/` |
| Schedules, retry, repeat, `Cron` | `scheduling.md`, `scheduling-retry.md` | `examples/06_schedule/` |
| Caching (`Cache`, `Effect.cached*`, `RcMap`) | `caching.md` | — |
| Batching, `Request` / `RequestResolver` | `request-resolver.md` | `examples/05_batching/` |
| Schema (codecs, classes, errors, JSON Schema) | `schema.md`, `schema-advanced.md` (JSON Schema, Model, SchemaBinary, Arbitrary, compilers), `migration/schema.md` | `examples/01_effect/02_schema/` |
| Config, `ConfigProvider` | `configuration.md`, `configuration-advanced.md` | — |
| Data types (Option, Result, Chunk, Duration, DateTime, ...) | `core-usage.md`, `data-types-advanced.md` | `examples/07_datetime/` |
| Equal, Hash, Order, Equivalence | `behavior-traits.md`, `migration/equality.md` | — |
| `Predicate` guards | `core-usage.md` | `examples/10_predicate/` |
| Logging, metrics, tracing, OTLP | `observability.md`, `observability-wiring.md`, `observability-examples.md` | `examples/08_observability/` |
| Platform services (FileSystem, Path, Terminal, Stdio, KeyValueStore, Crypto) | `platform-primitives.md` | — |
| Child processes (`effect/process`) | `platform-primitives.md` | `examples/60_child-process/` |
| HTTP client (`effect/http`) | `http-client.md` | `examples/50_http-client/` |
| HTTP server, HttpApi (`effect/http`, `effect/http-api`) | `http-server.md` | `examples/51_http-server/` |
| SQL (`effect/sql`, `@effect/sql-*`) | `sql.md` | `examples/40_sql/` |
| CLI (`effect/cli`) | `cli.md` | `examples/70_cli/` |
| AI (`effect/ai`, `@effect/ai-*`, MCP) | `ai.md` | `examples/71_ai/` |
| RPC / Cluster / Workflow / Reactivity | `rpc-cluster-workflow.md` | `examples/80_cluster/` |
| Testing, `@effect/vitest`, TestClock | `testing.md`, `testing-stack.md` | `examples/09_testing/`, `examples/51_http-server/20_testing.ts` |
| v3 → v4 migration | `migration.md`, `migration/*.md`, `migration/api-map/README.md` | — |
| Versions, stability tags, requirements | `versioning.md`, `changelog-4.0.md` | — |
| Debugging type errors / runtime surprises | `troubleshooting.md`, `exit-cause.md` | — |

Micro: removed in v4 (no guide); write `Effect` code instead.

## Upstream docs (Effect-TS/effect, `main` = 4.0.x)

Long-form guides shipped in `packages/effect/` (verified present in 4.0.1):

- Schema: https://github.com/Effect-TS/effect/blob/main/packages/effect/SCHEMA.md
- Config: https://github.com/Effect-TS/effect/blob/main/packages/effect/CONFIG.md
- HttpApi: https://github.com/Effect-TS/effect/blob/main/packages/effect/HTTPAPI.md
- MCP server: https://github.com/Effect-TS/effect/blob/main/packages/effect/MCP.md
- Arbitrary (property testing): https://github.com/Effect-TS/effect/blob/main/packages/effect/ARBITRARY.md
- Optic: https://github.com/Effect-TS/effect/blob/main/packages/effect/OPTIC.md
- LLM guide: https://github.com/Effect-TS/effect/blob/main/LLMS.md (local: `llms.md`)
- Examples source: https://github.com/Effect-TS/effect/tree/main/ai-docs/src (local: `examples/`)
- Migration: https://github.com/Effect-TS/effect/blob/main/MIGRATION.md and
  https://github.com/Effect-TS/effect/tree/main/migration
- Changelog: https://github.com/Effect-TS/effect/blob/main/packages/effect/CHANGELOG.md
- Per-package READMEs: `packages/<group>/<name>/README.md` (platform-*, sql-*, ai-*, atom-*, vitest, opentelemetry).

API reference (generated per package):

- https://effect.website/docs/v4/api/effect
- https://effect.website/docs/v4/api/<package> — e.g. `platform-node`,
  `platform-bun`, `sql-pg`, `ai-anthropic`, `ai-openai`, `vitest`,
  `opentelemetry`, `atom-react`.

Site: https://effect.website. Older pages and blog posts may show v3 APIs
(`Context.Tag`, `Effect.Service`, `@effect/platform`, `Either`); verify
against the v4 API reference or the installed `.d.ts` before copying.

## Routing checklist

- Start from task intent; open the primary guide and its official example.
- For a v3 symbol, grep `references/migration/api-map/`.
- For an `effect/<area>` module, check its stability tag before relying on it in
  a library (`references/versioning.md`).
- If guidance conflicts, prefer: installed source/JSDoc > `examples/` and
  `llms.md` > topic guides > website.
