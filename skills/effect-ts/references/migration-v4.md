# Effect v4 Migration Notes

Use this guide as a quick checklist when updating v3 docs or code to v4.

## High-level changes

- All Effect ecosystem packages share a single version number and release together.
- Many packages are consolidated into `effect`; platform/provider packages remain separate (`@effect/platform-*`, `@effect/sql-*`, `@effect/ai-*`, `@effect/atom-*`, `@effect/opentelemetry`, `@effect/vitest`).
- v4 introduces `effect/unstable/*` modules. APIs under `unstable/` can change in minor releases.

## Core renames and behavior changes

- `Context` → `ServiceMap`.
- `Context.Tag`, `Effect.Tag`, `Effect.Service` → `ServiceMap.Service`.
- `Context.Reference` / `FiberRef` → `ServiceMap.Reference` (access via `References.*`).
- `Effect.either` → `Effect.result` and `Either` → `Result`.
- `Effect.catchAll` → `Effect.catch`.
- `Effect.catchAllCause` → `Effect.catchCause`.
- `Effect.catchSome*` → `Effect.catchFilter` / `Effect.catchCauseFilter`.
- `Effect.fork` → `Effect.forkChild`.
- `Effect.forkDaemon` → `Effect.forkDetach`.
- `Runtime<R>` type removed; use `Effect.run*`. Use `Runtime.makeRunMain` only for custom process setup.
- `Effect.provide` shares layer memoization across calls. Use `Layer.fresh` or `Effect.provide(layer, { local: true })` for isolation.

## Before / After (common updates)

| v3 | v4 |
|---|---|
| `Context.Tag("Id")()` | `ServiceMap.Service()("Id")` |
| `Effect.Service` | `ServiceMap.Service` |
| `Context.Reference` | `ServiceMap.Reference` / `References.*` |
| `Effect.either` | `Effect.result` |
| `Either` | `Result` |
| `Effect.catchAll` | `Effect.catch` |
| `Effect.catchAllCause` | `Effect.catchCause` |
| `Effect.fork` | `Effect.forkChild` |
| `Effect.forkDaemon` | `Effect.forkDetach` |
| `FiberRef` | `References.*` |
| `Schema.decode` | `Schema.decode*Effect` |
| `Schema.encode` | `Schema.encode*Effect` |
| `Schema.encodedSchema` | `Schema.toEncoded` |
| `Schema.typeSchema` | `Schema.toType` |
| `Schema.Union(a, b)` | `Schema.Union([a, b])` |
| `Schema.Tuple(a, b)` | `Schema.Tuple([a, b])` |

## Yieldable

- Many v3 types no longer subtype `Effect`. Use `Effect.gen` with `yield*` and call module helpers (`Ref.get`, `Deferred.await`, `Fiber.join`).
- If you need to feed a Yieldable into Effect combinators, use `.asEffect()`.

## Cause

- `Cause` is flattened: reasons live in `cause.reasons`.
- Constructors/guards are reason-based (`Cause.fail`, `Cause.die`, `Cause.interrupt`, `Cause.has*`, `Cause.is*Reason`).

## Schema v4 (Codec)

- `Schema` is a Codec; use `Schema.revealCodec`, `Schema.toType`, `Schema.toEncoded`.
- `decode*` / `encode*` are now `decode*Effect` / `encode*Effect`.
- `Schema.decodeUnknownEffect` is the default boundary decoder.
- `Schema.Union` / `Schema.Tuple` / `Schema.TemplateLiteral` take arrays.
- `Schema.Record` is positional: `Schema.Record(key, value)`.
- `validate*` APIs are removed.

## HTTP / HttpApi (unstable)

- HTTP client/server modules are under `effect/unstable/http`.
- `HttpApi` lives under `effect/unstable/httpapi`.
- Other unstable modules currently include: `ai`, `cli`, `cluster`, `devtools`, `eventlog`, `jsonschema`, `observability`, `persistence`, `process`, `reactivity`, `rpc`, `schema`, `socket`, `sql`, `workflow`, `workers`.
- Provide runtime layers from `@effect/platform-*` packages.

## Unstable Modules Guidance

v4 ships with 18 unstable modules under `effect/unstable/*`. These APIs:
- May receive **breaking changes in minor releases** before graduating to stable APIs.
- Should **not be depended on in production** unless actively maintained alongside the Effect ecosystem.
- Serve as RFC-style exploration for future stable APIs.
- Will migrate to stable APIs once the design stabilizes.

**Recommendation:** Use unstable modules for:
- Prototyping and experimental features
- Internal tools and development utilities
- Code that can tolerate frequent updates

Avoid unstable modules for:
- Production services requiring stability
- Long-lived dependencies in published libraries
