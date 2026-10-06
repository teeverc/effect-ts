# v3 → v4 API map: effect/Iterable

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Iterable`

- `Iterable.flatMapNullable` -> `Iterable.flatMapNullishOr`: Direct nullish-terminology rename; it remains lazy and drops null or undefined mapper results.

- `Iterable.getLefts` -> `Iterable.getFailures`: Either became Result; this lazily extracts failure payloads.

- `Iterable.getRights` -> `Iterable.getSuccesses`: Either became Result; this lazily extracts success payloads.

- `Iterable.unsafeHead` -> `Iterable.headUnsafe`: Direct word-order rename; it still throws on an empty Iterable.
