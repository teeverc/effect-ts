---
name: effect-v4
description: "Effect v4 (stable, effect@4.0.x) development and v3 → v4 migration guidance. Use when writing, reviewing, or upgrading TypeScript code that uses Effect 4: Effect.gen/Effect.fn, Context.Service and Layer, Schema, errors and Cause, Stream, Schedule, fibers, HttpClient/HttpApi, SQL, CLI, AI, RPC/cluster, observability, @effect/vitest testing, and migrating v3 code (Context.Tag/Effect.Service, FiberRef, Either, catchAll, @effect/platform imports) to v4."
---

# Effect v4 (Stable) - Development & Migration

Effect 4.0 is the stable major release of Effect, published from
[Effect-TS/effect](https://github.com/Effect-TS/effect) (v4 was developed as
`effect-smol` during beta). This skill covers:

1. **Writing v4 code** with the patterns the Effect team recommends for AI agents (`references/llms.md`, `references/examples/`)
2. **Migrating v3 → v4**: official migration guides plus a per-module API rename map

## Ground Rules for v4 Code

- **Version alignment:** every Effect package shares one version. Use the same version of `effect` and every `@effect/*` package (e.g. `effect@4.0.1` + `@effect/platform-node@4.0.1`).
- **Imports:** `@effect/platform`, `rpc`, `cluster`, `cli`, `ai`, `sql`, `workflow`, `experimental` were merged into `effect`. Import from `effect/<area>`: `effect/http`, `effect/http-api`, `effect/rpc`, `effect/cluster`, `effect/workflow`, `effect/ai`, `effect/cli`, `effect/sql`, `effect/schema`, `effect/socket`, `effect/process`, `effect/encoding`, `effect/observability`, `effect/persistence`, `effect/reactivity`, `effect/net`, `effect/testing`. **`effect/unstable/*` paths no longer exist** (no compat exports).
- **Separate packages** remain only for platforms (`@effect/platform-node|bun|deno|browser`), SQL drivers (`@effect/sql-*`), AI providers (`@effect/ai-*`), `@effect/opentelemetry`, `@effect/atom-*`, `@effect/vitest`.
- **Stability:** untagged APIs follow semver. `@stability unstable` APIs may break in minor releases; `@stability experimental` may break in patches. Most area modules (http, ai, cli, sql, rpc, cluster, ...) are still `unstable` even though they moved out of `unstable/`.
- **Requirements:** TypeScript ≥ 5.9 (TS 7 recommended); `@effect/vitest` needs Vitest 5; `@effect/atom-react` needs React 19.

## Core Style (from the official agent guide)

```ts
import { Context, Effect, Layer, Schema } from "effect"

// Errors: Schema.TaggedError (or Data.TaggedError for non-serializable errors)
export class UserNotFound extends Schema.TaggedError<UserNotFound>()("UserNotFound", {
  id: Schema.String
}) {}

// Services: Context.Service with a static layer; implement methods with Effect.fn
export class Users extends Context.Service<Users, {
  findById(id: string): Effect.Effect<{ id: string; name: string }, UserNotFound>
}>()("myapp/users/Users") {
  static readonly layer = Layer.effect(
    Users,
    Effect.gen(function*() {
      const findById = Effect.fn("Users.findById")(function*(id: string) {
        if (id !== "1") {
          // Always `return yield*` when failing so TS knows control flow ends
          return yield* new UserNotFound({ id })
        }
        return { id, name: "Alice" }
      })
      return Users.of({ findById })
    })
  )
}

// Inline workflows: Effect.gen + .pipe combinators
const program = Effect.gen(function*() {
  const users = yield* Users
  const user = yield* users.findById("1")
  yield* Effect.log("found", user.name)
}).pipe(
  Effect.catchTag("UserNotFound", (e) => Effect.logWarning(`missing ${e.id}`)),
  Effect.provide(Users.layer)
)
```

- Inline code: `Effect.gen`. Reusable functions: `Effect.fn("name")` (traced) or `Effect.fnUntraced` (library/hot paths). Do not write functions that only wrap and return `Effect.gen`.
- With `Effect.fn`, pass extra combinators as additional arguments. **Do not `.pipe` an `Effect.fn`.** Annotate return types with `Effect.fn.Return<A, E, R>`.
- Model all untrusted data with `Schema`; never hand-roll parsers. Use `Predicate` (`Predicate.isObject`, `isString`, ...) instead of custom type guards.
- Use `DateTime` + `Clock` instead of `Date`/`Date.now()`; use `Context.Reference` for config/flags with defaults.
- Run at the edge only: `NodeRuntime.runMain` / `Layer.launch`; `ManagedRuntime` for framework integration.

## Quick Triage

### Official agent guide and examples (start here for idiomatic code)

- Official LLM/agent guide (style rules + topic index): `references/llms.md`
- Typechecked official examples by topic: `references/examples/` (e.g. `01_effect/03_services/`, `51_http-server/`, `71_ai/`)

### Building v4 code

- Effect basics, `Effect.fn`, Result, Option, Predicate, Array/Record: `references/core-usage.md`
- DateTime, Duration, BigDecimal, HashMap/HashSet, Redacted, Brand, Newtype: `references/data-types-advanced.md`
- Equal, Hash, Order, Equivalence: `references/behavior-traits.md`
- Generators, `Effect.fn`, Effectable: `references/generators.md`
- Expected errors, `catch*`, `catchReason`: `references/error-management.md`
- Cause inspection, `catchCause`, ErrorReporter: `references/error-tooling.md`
- Exit and flat Cause structure: `references/exit-cause.md`
- Services (`Context.Service`, `Context.Reference`): `references/dependency-management.md`
- Layer composition, memoization, LayerMap, `Layer.launch`: `references/layer-patterns.md`
- Scope, `acquireRelease`, Pool, RcRef/RcMap: `references/resource-management.md`
- Running programs, runtimes, ManagedRuntime: `references/runtime-execution.md`
- Fibers, forking, racing, FiberSet/FiberMap: `references/concurrency.md`
- References, Semaphore, Latch, Ref, Deferred: `references/concurrency-advanced.md`
- Schedules: `references/scheduling.md`; retries/backoff: `references/scheduling-retry.md`
- Streams, Queue, PubSub, STM (`Effect.tx`, `Tx*`): `references/streams-queues-stm.md`
- Sinks: `references/sink.md`
- Request batching: `references/request-resolver.md`
- Caching: `references/caching.md`
- Config: `references/configuration.md`, `references/configuration-advanced.md`
- Schema core: `references/schema.md`; JSON Schema, Model, SchemaBinary, compilers, Arbitrary: `references/schema-advanced.md`
- HttpClient: `references/http-client.md`
- HttpApi / HttpRouter servers and `HttpApiTest`: `references/http-server.md`
- FileSystem, Path, Terminal, Crypto, child processes: `references/platform-primitives.md`
- SQL (`effect/sql`, Model, drivers): `references/sql.md`
- CLI apps (`effect/cli`): `references/cli.md`
- AI (`effect/ai`, tools, chat, MCP): `references/ai.md`
- RPC, cluster, workflow, reactivity/atoms: `references/rpc-cluster-workflow.md`
- Logging, metrics, tracing, OTLP: `references/observability.md`, `references/observability-examples.md`, `references/observability-wiring.md`
- Testing (`@effect/vitest`, TestClock): `references/testing.md`, `references/testing-stack.md`
- Promise/async → Effect: `references/migration-async.md`
- Common pitfalls: `references/troubleshooting.md`
- Versions, stability tags, requirements: `references/versioning.md`
- What changed from beta to stable 4.0.x: `references/changelog-4.0.md`
- Topic → file routing map: `references/docs-index.md`

### Migrating from v3

- **Start here**: overview, import map, ordered checklist: `references/migration.md`
- Per-symbol API rename map (v3 symbol → v4 replacement + note): `references/migration/api-map/` — see `api-map/README.md` for the import map and file index, then `grep -rn "Effect.catchAll" references/migration/api-map/`
- Services (`Context.Tag` / `Effect.Service` → `Context.Service`): `references/migration/services.md`
- Error handling (`catchAll` → `catch`, etc.): `references/migration/error-handling.md`
- Cause flattening: `references/migration/cause.md`
- Schema v3 → v4: `references/migration/schema.md` (large; read in sections)
- Runtime removal and run functions: `references/migration/runtime.md`
- FiberRef → `Context.Reference`: `references/migration/fiberref.md`
- Forking renames and options: `references/migration/forking.md`
- Fiber keep-alive: `references/migration/fiber-keep-alive.md`
- Scope: `references/migration/scope.md`
- Layer memoization across `Effect.provide`: `references/migration/layer-memoization.md`
- Generators: `references/migration/generators.md`
- Effect subtyping, Effectable: `references/migration/yieldable.md`
- Equality: `references/migration/equality.md`

## Workflow

**Writing new code**

1. Identify the Effect APIs and concepts involved.
2. Search `references/llms.md` and `references/examples/` for them and read the matching example. Then open the topic reference above.
3. Follow the core style rules. Verify unfamiliar APIs in the installed `node_modules/effect` source/JSDoc rather than guessing, especially for `@stability unstable` modules.

**Migrating v3 code**

1. Identify which v3 APIs and packages are in scope (`grep` for `@effect/platform`, `Context.Tag`, `Effect.Service`, `FiberRef`, `Either`, `catchAll`, `Runtime`, `Schema` imports).
2. Align all package versions and rewrite imports using `references/migration/api-map/README.md`.
3. Open only the relevant guides in `references/migration/`, and look up individual symbols in `api-map/`.
4. Produce a v3 → v4 mapping with before/after code, call out behavior changes (keep-alive, layer memoization, structural equality, Cause shape, `partition` tuple order), and list test updates.
5. Give a short, ordered migration checklist for the code in question.

## Source of Truth

- Bundled references first; `references/llms.md`, `references/examples/` and `references/migration/` are copied verbatim from the official repository (4.0.1).
- For newer behavior, check the installed package's source/JSDoc, then `packages/*/CHANGELOG.md` in Effect-TS/effect, then <https://effect.website/docs>.
- Reject stale guidance: `effect/unstable/*` imports, `ServiceMap`, `Context.Tag`/`Effect.Service`, `FiberRef`, `Either`, `Micro`, `Effect.Yieldable`, `Schema.makeUnsafe`, independent `@effect/platform@0.x`-style versions. See `references/changelog-4.0.md`.

## Example Requests

- "Write a `Context.Service` with a layer and test it with `@effect/vitest`."
- "Build an HttpApi with auth middleware and test it with `HttpApiTest`."
- "Migrate this v3 `Effect.Service` + `FiberRef` + `catchAll` code to v4."
- "Our imports use `effect/unstable/http` from the beta; update them for 4.0."
- "Decode this API payload with Schema and map errors to tagged errors."
