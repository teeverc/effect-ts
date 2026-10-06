# v3 → v4 API map: effect/LogLevel

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/LogLevel`

- `LogLevel.All` -> `"All"`: V4 levels are string literals rather than branded objects; use the literal as both value and singleton type.

- `LogLevel.Debug` -> `"Debug"`: V4 levels are string literals rather than branded objects; use the literal as both value and singleton type.

- `LogLevel.Error` -> `"Error"`: V4 levels are string literals rather than branded objects; use the literal as both value and singleton type.

- `LogLevel.Fatal` -> `"Fatal"`: V4 levels are string literals rather than branded objects; use the literal as both value and singleton type.

- `LogLevel.Info` -> `"Info"`: V4 levels are string literals rather than branded objects; use the literal as both value and singleton type.

- `LogLevel.Literal` -> `LogLevel.LogLevel`: This is the all-level replacement after renaming Warning to Warn. LogLevel.Severity is narrower because it excludes All and None.

- `LogLevel.LogLevel` -> `LogLevel.LogLevel`: The name remains, but the representation is a string union and object fields such as \_tag, label, syslog, and ordinal are gone. Use toUpperCase() for labels and LogLevel.getOrdinal for ordering.

- `LogLevel.None` -> `"None"`: V4 levels are string literals rather than branded objects; use the literal as both value and singleton type.

- `LogLevel.Trace` -> `"Trace"`: V4 levels are string literals rather than branded objects; use the literal as both value and singleton type.

- `LogLevel.Warning` -> `"Warn"`: V4 renamed both the value and singleton type from Warning to the string literal Warn.

- `LogLevel.allLevels` -> `LogLevel.values`: Use the ordered v4 array of all levels, including All and None.

- `LogLevel.fromLiteral` -> `literal === "Warning" ? "Warn" : literal`: No constructor is needed because v4 levels are strings. Normalize the renamed Warning literal to Warn; all other v3 literals pass through.

- `LogLevel.greaterThan` -> `LogLevel.isGreaterThan`: Direct rename; ordering remains severity ordering.

- `LogLevel.greaterThanEqual` -> `LogLevel.isGreaterThanOrEqualTo`: Direct rename.

- `LogLevel.lessThan` -> `LogLevel.isLessThan`: Direct rename.

- `LogLevel.lessThanEqual` -> `LogLevel.isLessThanOrEqualTo`: Direct rename.

- `LogLevel.locally` -> `Effect.provideService(effect, References.CurrentLogLevel, level)`: Current log level is now a reference. For threshold configuration, including All or None, provide References.MinimumLogLevel instead.
