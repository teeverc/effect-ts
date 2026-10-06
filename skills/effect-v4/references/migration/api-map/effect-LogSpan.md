# v3 → v4 API map: effect/LogSpan

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/LogSpan`

- `LogSpan.LogSpan` -> `readonly [label: string, timestamp: number]`: The module was removed. Active log spans are tuples in References.CurrentLogSpans; ordinary callers should prefer Effect.withLogSpan.

- `LogSpan.make` -> `[label, startTime] as const`: Construct the tuple directly, or use Effect.withLogSpan so Effect obtains the timestamp and scopes the span.

- `LogSpan.render` -> `custom tuple formatter`: No public standalone renderer remains. Built-in loggers format span tuples internally; custom formatters can render label and elapsed milliseconds themselves.
