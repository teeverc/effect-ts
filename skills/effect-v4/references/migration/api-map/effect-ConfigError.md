# v3 → v4 API map: effect/ConfigError

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/ConfigError`

- `ConfigError.And` -> `SchemaIssue.Composite`: The ConfigError boolean ADT was removed; combined schema failures are represented inside Config.ConfigError.cause as SchemaIssue.Composite.

- `ConfigError.ConfigError` -> `Config.ConfigError`: Config errors are now a class in effect/Config wrapping either ConfigProvider.SourceError or Schema.SchemaError.

- `ConfigError.ConfigError.Proto` -> `Config.ConfigError`: The public prototype interface was removed; use the Config.ConfigError class.

- `ConfigError.ConfigError.Reducer` -> `none`: The ConfigError-specific reducer API was removed; inspect ConfigError.cause and recurse over SchemaError.issue when structured handling is required.

- `ConfigError.ConfigErrorReducer` -> `none`: The ConfigError-specific reducer API was removed; inspect ConfigError.cause and recurse over SchemaError.issue when structured handling is required.

- `ConfigError.ConfigErrorTypeId` -> `error instanceof Config.ConfigError`: The marker is gone because ConfigError is a class in v4.

- `ConfigError.InvalidData` -> `new Config.ConfigError(new Schema.SchemaError(issue))`: Invalid configuration is now expressed as a SchemaIssue wrapped by SchemaError and Config.ConfigError.

- `ConfigError.MissingData` -> `none`: There is no public missing-data error variant. A required absent config ultimately fails with a SchemaError, while Config.withDefault and Config.option handle semantic absence before it enters the public Effect error channel.

- `ConfigError.Options` -> `none`: The shared constructor options type was removed; ConfigProvider.SourceError accepts message and optional cause, while Schema issues have issue-specific constructors.

- `ConfigError.Or` -> `SchemaIssue.AnyOf`: The ConfigError boolean ADT was removed. Schema union failures can use SchemaIssue.AnyOf inside Config.ConfigError.cause; Config.orElse adopts the fallback's result instead of combining failed attempts.

- `ConfigError.SourceUnavailable` -> `new ConfigProvider.SourceError({ message, cause })`: Source failures moved to effect/ConfigProvider and are wrapped by Config.ConfigError when a Config is parsed.

- `ConfigError.Unsupported` -> `none`: The variant was removed; use a SchemaError for unsupported input or ConfigProvider.SourceError for source capability failures.

- `ConfigError.isAnd` -> `error.cause.issue._tag === "Composite"`: After narrowing cause with Schema.isSchemaError, inspect the SchemaIssue tag; the old And node no longer exists.

- `ConfigError.isConfigError` -> `error instanceof Config.ConfigError`: ConfigError is a class in v4.

- `ConfigError.isInvalidData` -> `Schema.isSchemaError(error.cause)`: Parsing and validation failures are SchemaError causes; inspect the contained SchemaIssue for finer classification.

- `ConfigError.isMissingData` -> `none`: Do not infer semantic absence from a SchemaIssue. Use Config.withDefault or Config.option; they recover missing data, including Config.all groups with missing children, while preserving successful undefined and propagating validation and source errors.

- `ConfigError.isMissingDataOnly` -> `Config.withDefault / Config.option`: The public classifier was removed. These combinators handle absence without recursively classifying SchemaIssue values; invalid input and source errors still propagate.

- `ConfigError.isOr` -> `error.cause.issue._tag === "AnyOf"`: After narrowing cause with Schema.isSchemaError, AnyOf identifies schema union failures. Config.orElse does not combine failed attempts; it propagates only the fallback's error.

- `ConfigError.isSourceUnavailable` -> `error.cause instanceof ConfigProvider.SourceError`: Provider source failures now use the ConfigProvider.SourceError class.

- `ConfigError.isUnsupported` -> `none`: The Unsupported variant was removed; report unsupported custom decoding through a SchemaError or source failures through ConfigProvider.SourceError.

- `ConfigError.prefixed` -> `SchemaIssue.Pointer`: Represent path context by wrapping the underlying SchemaIssue in a Pointer before constructing SchemaError.

- `ConfigError.reduceWithContext` -> `none`: The specialized fold was removed; branch on ConfigError.cause, then recurse over the public SchemaIssue union if a fold is needed.
