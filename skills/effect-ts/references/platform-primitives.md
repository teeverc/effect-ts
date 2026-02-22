# Platform Primitives (Command, FileSystem, Path, Terminal, KeyValueStore)

Use this guide when writing Effect code that interacts with OS/process/file/terminal/storage boundaries.

## Mental model

- Keep platform effects in adapters and expose typed domain services.
- Use platform modules through Effect services so behavior is replaceable in tests.
- Keep path handling platform-safe by delegating joins/normalization to `Path`.
- Treat command execution and file IO as boundary concerns with explicit failure types.

## Patterns

- Prefer dependency-injected wrappers around `Command`/`FileSystem`/`Terminal`.
- Use `KeyValueStore` for simple persistence and caching boundaries.
- Keep serialization/deserialization and schema validation near storage boundaries.
- Use scoped resources for handles/streams tied to lifecycle.
- Convert platform exceptions to typed domain errors at adapter boundaries.

## Adapter sketch

```ts
import { Effect } from "effect"

interface ConfigStore {
  readonly load: (key: string) => Effect.Effect<string, "NotFound" | "StoreError">
}
```

## Pitfalls

- Building paths with string concatenation instead of `Path`.
- Executing commands without timeout/cancellation strategy.
- Mixing domain logic with file/terminal side effects.
- Relying on global mutable process state in tests.
- Writing logs/data without redaction for sensitive values.

## Docs

- `https://effect.website/docs/platform/introduction/`
- `https://effect.website/docs/platform/command/`
- `https://effect.website/docs/platform/file-system/`
- `https://effect.website/docs/platform/path/`
- `https://effect.website/docs/platform/key-value-store/`
- `https://effect.website/docs/platform/terminal/`
- `https://effect.website/docs/platform/platformlogger/`
- `https://effect.website/docs/platform/runtime/`
