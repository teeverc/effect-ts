# Effect v4 Migration Notes

Effect 4.0 is stable (`effect@4.0.x`, published from
[Effect-TS/effect](https://github.com/Effect-TS/effect)). For full v4 guidance
and the v3 → v4 migration guides, use the `effect-v4` skill
(`npx skills add teeverc/effect-ts --skill effect-v4`) or the official
[MIGRATION.md](https://github.com/Effect-TS/effect/blob/main/MIGRATION.md).

## Key facts when planning a migration

- All Effect packages share one version; bump `effect` and every `@effect/*` package together.
- `@effect/platform`, `@effect/rpc`, `@effect/cluster`, `@effect/cli`, `@effect/ai`, `@effect/sql`, `@effect/workflow` and `@effect/experimental` were merged into `effect`. Import from `effect/<area>` (`effect/http`, `effect/http-api`, `effect/rpc`, `effect/sql`, `effect/cli`, `effect/ai`, ...). Beta-era `effect/unstable/*` paths no longer exist.
- Separate packages remain for platforms (`@effect/platform-*`), SQL drivers (`@effect/sql-*`), AI providers (`@effect/ai-*`), `@effect/opentelemetry`, `@effect/atom-*` and `@effect/vitest`.
- APIs tagged `@stability unstable` may break in minor releases; untagged APIs follow semver.
- Requires TypeScript 5.9+.

## Most common renames

| v3 | v4 |
| --- | --- |
| `Context.Tag` / `Effect.Service` | `Context.Service` |
| `FiberRef` | `Context.Reference` |
| `Either` | `Result` |
| `Effect.catchAll` | `Effect.catch` |
| `Effect.catchAllCause` | `Effect.catchCause` |
| `Effect.fork` | `Effect.forkChild` |
| `Effect.forkDaemon` | `Effect.forkDetach` |
| `Runtime<R>` | removed; carry a `Context` and use `Effect.run*With`, or use `ManagedRuntime` |
| `Micro` | removed; use `Effect` |
| `TRef`, `TQueue`, ... (STM) | `TxRef`, `TxQueue`, ... with `Effect.tx` |
| `@effect/platform/HttpClient` | `effect/http/HttpClient` |
| `@effect/schema` / v3 `Schema` | v4 `Schema` (new implementation; see the Schema migration guide) |
