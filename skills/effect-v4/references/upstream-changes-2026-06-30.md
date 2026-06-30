# Upstream Changes Through 2026-06-30

Research source: `Effect-TS/effect-smol` `main`, cloned June 30, 2026. The
local skill refresh on April 9, 2026 was compared against upstream history.

Commit range reviewed:

- 751 commits since 2026-02-22, the original stale-guidance baseline recorded
  in this skill.
- 317 commits since 2026-04-09, the local "latest beta changes" skill commit.
- Current package changelog reached `effect@4.0.0-beta.92`.

## Highest-impact guidance changes

- `Effect.Yieldable` was removed. Use `Effectable.Class` /
  `Effectable.Prototype` only for custom executable values. Use explicit module
  functions for ordinary handles (`Ref.get`, `Deferred.await`, `Fiber.join`).
- `SchemaParser.makeUnsafe` was renamed to `SchemaParser.make`.
- `Schema.asserts` and `SchemaParser.asserts` now assert an existing input
  directly with `(schema, input)`; `Schema.Codec.ToAsserts` was removed.
- `Schema.Void` now models ignored `void` returns: present input decodes to
  `undefined`. Use `Schema.Undefined` for exact-undefined validation.
- `Schema.toCodecStringTree` no longer takes `keepDeclarations`.
- `Schema.toType`, `Schema.toEncoded`, `Schema.toCodecJson`, and
  `Schema.toCodecStringTree` expose their source schema via `.schema`.
- Schema and SchemaParser boundary APIs now preserve full causes in Effect/Exit
  forms and normalize schema-only failures in sync, promise, option, and result
  forms.
- Decoding defaults can require services, and constructor/decoding defaults can
  fail with `SchemaError`.
- `HttpApiBuilder.layer(Api)` plus `HttpRouter.serve` is the current server
  assembly pattern in upstream docs.
- `HttpApiTest` exists for endpoint testing, `HttpApiSecurity.http` supports
  custom HTTP security schemes, and HttpApi supports streaming responses.

## New or changed APIs worth using

- Core: `Effect.firstSuccessOf`, `Effect.transposeOption`,
  custom-error callbacks for `Effect.fromOption`, `Effect.abortSignal`,
  `Effect.acquireDisposable`, `Random.choice`.
- Schema: `Schema.DurationFromString`, `Schema.GUID`, `Schema.encodeKeys`,
  `Schema.Error` / `Schema.Defect` refactors, `Schema.toCodecArrayFromSingle`,
  `Schema.ConstraintDecoder`, class `.extend(Struct)`.
- Config: `Config.literals`, fixed `Config.schema` missing arrays, fixed config
  path composition.
- Scheduling: `Schedule.tap`, corrected `Schedule.andThenResult` output
  polarity, `Cron.next` overflow fix.
- Streams/channels: `Stream.broadcastN`, options-based `Stream.toQueue`,
  corrected `Stream.splitLines` CR/final-line behavior, fixed UTF-8 chunk
  boundary decoding.
- Platform/unstable: platform `Crypto` service, `Terminal` rows, browser
  IndexedDB KeyValueStore layer, SQL PGlite package.
- Observability: OTLP env configuration layer, resource precedence changes,
  cause rendering in tracer exception events.
- Models/SQL/cluster: `Model.GeneratedByDb` replaces `Model.Generated`;
  `SqlModel` repositories support soft deletes and unique constraints.

## Stale guidance to reject

- `ServiceMap.*` instead of `Context.*`.
- `Schema.makeUnsafe` or `SchemaParser.makeUnsafe`.
- `Effect.Yieldable`, `.asEffect()`, or broad "many values are Yieldable"
  migration text.
- `Schema.Codec.ToAsserts`.
- `Random.nextUUIDv4`; use the platform `Crypto` service for cryptographic UUIDs.
- `Model.Generated`; use `Model.GeneratedByDb`.
- `Schema.Void` as exact `undefined` validation.
