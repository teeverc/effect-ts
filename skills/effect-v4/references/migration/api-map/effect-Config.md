# v3 → v4 API map: effect/Config

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Config`

- `Config.Config` -> `Config.Config`: The model remains a yieldable Effect and exposes parse(provider). Compose logical lookup paths with Config.schema(..., path) and Config.nested; parsing no longer accepts a public path prefix.

- `Config.Config.IsPlainObject` -> `none`: This private conditional helper is no longer exposed; use Config.Wrap for the public recursive wrapping contract.

- `Config.Config.Primitive` -> `Schema.Constraint`: Primitive descriptions and parsers were replaced by Schema codecs consumed through Config.schema.

- `Config.Config.Variance` -> `none`: Config now carries its result type directly through Effect and has no public variance interface.

- `Config.ConfigTypeId` -> `Config.isConfig`: The Config marker is private in v4; use the public guard for runtime narrowing.

- `Config.LiteralValue` -> `SchemaAST.LiteralValue`: Use the literal value type shared by v4 Schema constructors.

- `Config.all` -> `Config.all`: Combine an iterable or record of Config values. If any child is absent and none fail, Config.withDefault replaces the whole group and Config.option returns None. Apply defaults to individual children to preserve other supplied values.

- `Config.array` -> `Config.Array(valueSchema, path)`: Array parsing is schema-based in v4; rebuild the element Config as a Schema and pass it with the optional path to Config.Array.

- `Config.boolean` -> `Config.Boolean`: Direct constructor rename.

- `Config.branded` -> `Config.schema(schema.pipe(Schema.brand(brand)), path)`: Brand validation moved to Schema; define the branded schema and construct the Config with Config.schema.

- `Config.chunk` -> `Config.schema(Schema.Chunk(valueSchema), path)`: Collection parsing is schema-based in v4; use Schema.Chunk when a Chunk result is still required.

- `Config.date` -> `Config.Date`: Direct constructor rename.

- `Config.duration` -> `Config.Duration`: Direct constructor rename.

- `Config.fail` -> `Config.fail`: The v4 constructor takes a ConfigProvider.SourceError or Schema.SchemaError instead of a message; wrap the failure in the appropriate cause.

- `Config.hashMap` -> `Config.schema(Schema.HashMap(Schema.String, valueSchema), path)`: HashMap parsing is schema-based in v4; replace the child Config with its value Schema.

- `Config.hashSet` -> `Config.schema(Schema.HashSet(valueSchema), path)`: HashSet parsing is schema-based in v4; replace the child Config with its value Schema.

- `Config.integer` -> `Config.Int`: Renamed to the shorter v4 integer constructor using the PascalCase constructor convention.

- `Config.literal` -> `Config.Literals(literals, path)`: The v3 curried variadic constructor became Config.Literals with an array and inline path; use Config.Literal for one value.

- `Config.logLevel` -> `Config.LogLevel`: Direct constructor rename.

- `Config.mapAttempt` -> `Config.mapEffect`: Catch exceptions explicitly and return an Effect failure containing Config.ConfigError; mapEffect is Effect-based in v4.

- `Config.mapOrFail` -> `Config.mapEffect`: Renamed to match the effectful mapping convention used throughout the library.

- `Config.nonEmptyString` -> `Config.NonEmptyString`: Direct constructor rename.

- `Config.number` -> `Config.Number`: Direct constructor rename; use Config.Finite when NaN and infinities must be rejected.

- `Config.option` -> `Config.option`: Returns None for an absent config, including an all group with a missing child and no failed children. Successfully decoded undefined is wrapped in Some; validation and source errors still propagate.

- `Config.orElse` -> `Config.orElse`: The fallback receives Config.ConfigError and its result replaces the original failure. Unlike v3, failed attempts are not combined: an absent fallback can be handled by a later withDefault or option even if the original input was invalid.

- `Config.orElseIf` -> `Config.orElse`: The fallback now receives Config.ConfigError; test it in the callback and re-fail with Config.fail(error.cause) when the predicate is false.

- `Config.port` -> `Config.Port`: Direct constructor rename.

- `Config.primitive` -> `Config.schema(customSchema, path)`: Custom primitive parsing moved to Schema codecs; express decoding and diagnostics in a Schema, then pass it to Config.schema. Its canonical StringTree encoding must expose a concrete shape; opaque encodings such as Schema.Any or Schema.Unknown are not supported.

- `Config.redacted` -> `Config.Redacted`: The string/path overload remains; replace the v3 Config argument overload with Config.map(config, Redacted.make).

- `Config.repeat` -> `Config.Array(valueSchema, path)`: Repeated values use the Config.Array constructor, which accepts structural arrays and flat separated input.

- `Config.secret` -> `Config.Redacted`: Secret was removed in favor of Redacted; this constructor returns Redacted\<string\>.

- `Config.string` -> `Config.String`: Direct constructor rename.

- `Config.succeed` -> `Config.succeed`: Unchanged.

- `Config.suspend` -> `Config.schema(Schema.suspend(schemaThunk), path)`: General Config suspension was removed; model recursive parsing with a suspended Schema before constructing the Config.

- `Config.sync` -> `Config.succeed(undefined).pipe(Config.map(() => thunk()))`: The dedicated lazy constant constructor was removed; mapping a constant Config preserves evaluation at parse time.

- `Config.url` -> `Config.URL`: Direct constructor rename.

- `Config.validate` -> `Config.schema(schema.check(check), path)`: Validation moved to Schema checks; attach the predicate and message to the Schema used by Config.schema.

- `Config.withDefault` -> `Config.withDefault`: Defaults absent configs, including all groups with missing children and no failed children. A group default replaces the whole group; use defaults on children to preserve other supplied values. Validation and source errors still propagate.

- `Config.withDescription` -> `Config.schema(schema.annotate({ description }), path)`: Config descriptions moved to Schema annotations in v4.

- `Config.zip` -> `Config.all([self, that])`: Use the tuple overload of Config.all.

- `Config.zipWith` -> `Config.all([self, that]).pipe(Config.map(([a, b]) => f(a, b)))`: Combine both configs with Config.all, then map the tuple.
