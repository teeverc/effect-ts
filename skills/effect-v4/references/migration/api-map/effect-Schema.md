# v3 → v4 API map: effect/Schema

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Schema`

- `Schema.Annotable` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Annotable.All` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Annotable.Any` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Annotable.Self` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.AnnotableClass` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.AnnotableDeclare` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Annotations` -> `Schema.Annotations`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Annotations.Doc` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Annotations.Filter` -> `Schema.Annotations.Filter`: Use the v4 filter annotation shape. arbitraryConstraint uses Schema.Annotations.ToArbitrary.FilterConstraint; the earlier v4 ToArbitrary.Constraint name was removed.

- `Schema.Annotations.GenericSchema` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Annotations.Schema` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Any` -> `Schema.Any`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Array$` -> `Schema.$Array`: Use the renamed v4 constructor result interface.

- `Schema.ArrayEnsure` -> `Schema.ArrayEnsure`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.ArrayFormatterIssue` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.BetweenBigDecimalSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.BetweenBigIntSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.BetweenDateSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.BetweenDurationSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.BetweenSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.BigDecimal` -> `Schema.BigDecimalFromString`: Use the string-to-BigDecimal codec; v4 `BigDecimal` is the self schema.

- `Schema.BigDecimalFromNumber` -> `none`: No built-in number-to-BigDecimal codec remains; compose `decodeTo` with a `SchemaGetter` conversion.

- `Schema.BigDecimalFromSelf` -> `Schema.BigDecimal`: The self schema dropped the `FromSelf` suffix.

- `Schema.BigInt` -> `Schema.BigIntFromString`: Use the string-to-bigint codec; v4 `BigInt` is the self schema.

- `Schema.BigIntFromNumber` -> `none`: No built-in number-to-bigint codec remains; compose `decodeTo` with a checked `SchemaGetter` conversion.

- `Schema.BigIntFromSelf` -> `Schema.BigInt`: The self schema dropped the `FromSelf` suffix.

- `Schema.Boolean` -> `Schema.Boolean`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.BooleanFromString` -> `none`: No built-in string-to-boolean codec remains; use `decodeTo` with an explicit `SchemaGetter` transformation.

- `Schema.BooleanFromUnknown` -> `Schema.Boolean`: Use the boolean schema and perform any coercion explicitly before decoding.

- `Schema.BrandSchema` -> `Schema.brand`: Use the schema returned by the v4 `brand` combinator and infer its concrete type. The brand exists only in the TypeScript type and cannot be inspected through the runtime AST.

- `Schema.BrandSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.Capitalize` -> `Schema.String.pipe(Schema.decodeTo(Schema.String.check(Schema.isCapitalized()), SchemaTransformation.capitalize()))`: Rebuild the capitalization transformation with `decodeTo`.

- `Schema.Capitalized` -> `Schema.String.check(Schema.isCapitalized())`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.CapitalizedSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.Cause` -> `Schema.toCodecJson(Schema.Cause(error, defect))`: Use the derived JSON codec to preserve v3's encoded Cause representation; v4 `Cause` itself is the self schema.

- `Schema.CauseEncoded` -> `Schema.CauseIso`: Use the v4 Cause JSON/iso representation type.

- `Schema.CauseFromSelf` -> `Schema.Cause`: The self schema dropped the `FromSelf` suffix.

- `Schema.Char` -> `Schema.Char`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Chunk` -> `Schema.toCodecJson(Schema.Chunk(value))`: Use the derived JSON codec to preserve v3's array-to-Chunk behavior; v4 `Chunk` itself is the self schema.

- `Schema.ChunkFromSelf` -> `Schema.Chunk`: The self schema dropped the `FromSelf` suffix.

- `Schema.Class` -> `Schema.Class`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Config` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.Data` -> `none`: Remove this wrapper. v4 structural equality works on ordinary decoded objects.

- `Schema.DataFromSelf` -> `none`: Remove this wrapper. v4 structural equality works on ordinary decoded objects.

- `Schema.Date` -> `Schema.DateFromString`: Use `DateFromString`; v4 `Date` is the self schema.

- `Schema.DateFromNumber` -> `Schema.DateFromMillis`: Rename the milliseconds-to-Date codec.

- `Schema.DateFromSelf` -> `Schema.Date`: The self schema dropped the `FromSelf` suffix.

- `Schema.DateFromSelfSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.DateFromString` -> `Schema.DateFromString`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.DateTimeUtc` -> `Schema.DateTimeUtcFromString`: Use the string codec; v4 `DateTimeUtc` is the self schema.

- `Schema.DateTimeUtcFromDate` -> `Schema.DateTimeUtcFromDate`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.DateTimeUtcFromNumber` -> `Schema.DateTimeUtcFromMillis`: Rename the milliseconds-to-DateTime codec.

- `Schema.DateTimeUtcFromSelf` -> `Schema.DateTimeUtc`: The self schema dropped the `FromSelf` suffix.

- `Schema.DateTimeZoned` -> `Schema.DateTimeZonedFromString`: Use the string codec; v4 `DateTimeZoned` is the self schema.

- `Schema.DateTimeZonedFromSelf` -> `Schema.DateTimeZoned`: The self schema dropped the `FromSelf` suffix.

- `Schema.Defect` -> `Schema.Defect`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Duration` -> `Schema.DurationFromString`: Use the string codec; v4 `Duration` is the self schema.

- `Schema.DurationEncoded` -> `Schema.Duration["Iso"]`: Use the v4 Duration iso representation type.

- `Schema.DurationFromMillis` -> `Schema.DurationFromMillis`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.DurationFromNanos` -> `Schema.DurationFromNanos`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.DurationFromSelf` -> `Schema.Duration`: The self schema dropped the `FromSelf` suffix.

- `Schema.Either` -> `Schema.Result`: `Either` was renamed to `Result`; pass success and failure schemas positionally.

- `Schema.EitherEncoded` -> `Schema.ResultIso`: Use the v4 Result iso representation type.

- `Schema.EitherFromSelf` -> `Schema.Result`: `Either` was renamed to `Result` in v4.

- `Schema.EitherFromUnion` -> `Schema.Result`: `Either` was renamed to `Result`; use its tagged Result representation.

- `Schema.Element` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Element.Token` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.EndsWithSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.Enums` -> `Schema.Enum`: Rename the enum constructor and pass the enum object. Numeric members must be finite; NaN and positive or negative Infinity throw at construction.

- `Schema.EnumsDefinition` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.Exit` -> `Schema.toCodecJson(Schema.Exit(value, error, defect))`: Use the derived JSON codec to preserve v3's encoded Exit representation; v4 `Exit` itself is the self schema.

- `Schema.ExitEncoded` -> `Schema.ExitIso`: Use the v4 Exit iso representation type.

- `Schema.ExitFromSelf` -> `Schema.Exit`: The self schema dropped the `FromSelf` suffix.

- `Schema.FiberId` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.FiberIdEncoded` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.FiberIdFromSelf` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.FilterIssue` -> `Schema.FilterIssue`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.FilterOutput` -> `Schema.FilterOutput`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Finite` -> `Schema.Finite`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.FiniteSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.FromPropertySignature` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.GreaterThanBigDecimalSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.GreaterThanBigIntSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.GreaterThanDateSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.GreaterThanDurationSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.GreaterThanOrEqualToBigDecimalSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.GreaterThanOrEqualToBigIntSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.GreaterThanOrEqualToDateSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.GreaterThanOrEqualToDurationSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.GreaterThanOrEqualToSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.GreaterThanSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.HashMap` -> `Schema.toCodecJson(Schema.HashMap(key, value))`: Pass key and value positionally and use the derived JSON codec to preserve v3's entry-array encoding.

- `Schema.HashMapFromSelf` -> `Schema.HashMap`: The self schema dropped the `FromSelf` suffix; pass key and value positionally.

- `Schema.HashSet` -> `Schema.toCodecJson(Schema.HashSet(value))`: Use the derived JSON codec to preserve v3's array-to-HashSet behavior.

- `Schema.HashSetFromSelf` -> `Schema.HashSet`: The self schema dropped the `FromSelf` suffix.

- `Schema.IncludesSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.IndexSignature` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.IndexSignature.Context` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.IndexSignature.Encoded` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.IndexSignature.NonEmptyRecords` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.IndexSignature.Record` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.IndexSignature.Type` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.InstanceOfSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.Int` -> `Schema.Int`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.IntSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.ItemsCountSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.JsonNumber` -> `Schema.Finite`: Use the finite-number schema for JSON-compatible numbers.

- `Schema.JsonNumberSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LeftEncoded` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.LengthSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LessThanBigDecimalSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LessThanBigIntSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LessThanDateSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LessThanDurationSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LessThanOrEqualToBigDecimalSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LessThanOrEqualToBigIntSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LessThanOrEqualToDateSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LessThanOrEqualToDurationSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LessThanOrEqualToSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.LessThanSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.List` -> `none`: The List schema was removed; migrate the model to `Schema.Array` or declare a custom List codec.

- `Schema.ListFromSelf` -> `none`: The List self schema was removed; migrate to arrays or use `Schema.declare` for List values.

- `Schema.Literal` -> `Schema.Literal / Schema.Literals`: Use `Literal(value)` for one non-null literal, `Null` for null, and `Literals([...])` for several literals.

- `Schema.Lowercase` -> `Schema.String.pipe(Schema.decodeTo(Schema.String.check(Schema.isLowercased()), SchemaTransformation.toLowerCase()))`: Rebuild the lowercase transformation with `decodeTo`.

- `Schema.Lowercased` -> `Schema.String.check(Schema.isLowercased())`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.LowercasedSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.MakeOptions` -> `Schema.MakeOptions`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Map` -> `Schema.toCodecJson(Schema.ReadonlyMap(key, value))`: Use `ReadonlyMap` with positional arguments and derive its JSON codec; mutable Map-specific schema types were removed.

- `Schema.Map$` -> `Schema.$ReadonlyMap`: Use the renamed v4 constructor result interface.

- `Schema.MapFromRecord` -> `none`: No direct record-to-Map codec remains; compose `Record` and `ReadonlyMap` with an explicit `decodeTo` transformation.

- `Schema.MapFromSelf` -> `Schema.ReadonlyMap`: Use the readonly Map self schema with positional key and value arguments.

- `Schema.MaxItemsSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.MaxLengthSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.MinItemsSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.MinLengthSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.MultipleOfSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.Negative` -> `Schema.Number.check(Schema.isLessThan(0))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NegativeBigDecimalFromSelf` -> `Schema.BigDecimal.check(Schema.isLessThanBigDecimal(BigDecimal.fromNumber(0)))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NegativeBigDecimalSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.NegativeBigInt` -> `Schema.BigIntFromString.check(Schema.isLessThanBigInt(0n))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NegativeBigIntFromSelf` -> `Schema.BigInt.check(Schema.isLessThanBigInt(0n))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.Never` -> `Schema.Never`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.NonEmptyArray` -> `Schema.NonEmptyArray`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.NonEmptyArrayEnsure` -> `none`: No direct replacement remains; explicitly decode a single value or array to `Schema.NonEmptyArray`.

- `Schema.NonEmptyChunk` -> `Schema.toCodecJson(Schema.Chunk(value).check(Schema.isMinLength(1)))`: Use a checked Chunk JSON codec.

- `Schema.NonEmptyChunkFromSelf` -> `Schema.Chunk(value).check(Schema.isMinLength(1))`: Use the Chunk self schema with a minimum-length check.

- `Schema.NonEmptyString` -> `Schema.NonEmptyString`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.NonEmptyTrimmedString` -> `Schema.Trimmed.check(Schema.isNonEmpty())`: Compose the trimmed schema with the non-empty check.

- `Schema.NonNaN` -> `Schema.Number.check(Schema.makeFilter((n) => !Number.isNaN(n)))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NonNaNSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.NonNegative` -> `Schema.Number.check(Schema.isGreaterThanOrEqualTo(0))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NonNegativeBigDecimalFromSelf` -> `Schema.BigDecimal.check(Schema.isGreaterThanOrEqualToBigDecimal(BigDecimal.fromNumber(0)))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NonNegativeBigDecimalSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.NonNegativeBigInt` -> `Schema.BigIntFromString.check(Schema.isGreaterThanOrEqualToBigInt(0n))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NonNegativeBigIntFromSelf` -> `Schema.BigInt.check(Schema.isGreaterThanOrEqualToBigInt(0n))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NonNegativeInt` -> `Schema.Number.check(Schema.isInt(), Schema.isGreaterThanOrEqualTo(0))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NonPositive` -> `Schema.Number.check(Schema.isLessThanOrEqualTo(0))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NonPositiveBigDecimalFromSelf` -> `Schema.BigDecimal.check(Schema.isLessThanOrEqualToBigDecimal(BigDecimal.fromNumber(0)))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NonPositiveBigDecimalSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.NonPositiveBigInt` -> `Schema.BigIntFromString.check(Schema.isLessThanOrEqualToBigInt(0n))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.NonPositiveBigIntFromSelf` -> `Schema.BigInt.check(Schema.isLessThanOrEqualToBigInt(0n))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.Not` -> `none`: The exclusion constructor was removed; express the accepted alternatives directly or add a `Schema.check`.

- `Schema.Null` -> `Schema.Null`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.NullOr` -> `Schema.NullOr`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.NullishOr` -> `Schema.NullishOr`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Number` -> `Schema.Number`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.NumberFromString` -> `Schema.NumberFromString`: Decoding now uses JavaScript number coercion: unlike v3, `"abc"` decodes to `NaN`, while `""` and whitespace-only strings decode to `0`. `FiniteFromString` rejects non-finite results, so it rejects invalid strings but also the `"NaN"` and infinity spellings accepted by v3; it still decodes blank strings to `0`. Use a custom codec to preserve the v3 acceptance rules exactly.

- `Schema.Object` -> `Schema.ObjectKeyword`: Rename the object keyword schema.

- `Schema.Option` -> `Schema.toCodecJson(Schema.Option(value))`: Use the derived JSON codec to preserve v3's tagged Option encoding; v4 `Option` itself is the self schema.

- `Schema.OptionEncoded` -> `Schema.OptionIso`: Use the v4 Option iso representation type.

- `Schema.OptionFromNonEmptyTrimmedString` -> `Schema.Trimmed.check(Schema.isNonEmpty()).pipe(Schema.decodeTo(Schema.Option(Schema.String), ...))`: Rebuild the empty-string-to-None conversion explicitly with `decodeTo`.

- `Schema.OptionFromNullOr` -> `Schema.OptionFromNullOr`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.OptionFromNullishOr` -> `Schema.OptionFromNullishOr`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.OptionFromSelf` -> `Schema.Option`: The self schema dropped the `FromSelf` suffix.

- `Schema.OptionFromUndefinedOr` -> `Schema.OptionFromUndefinedOr`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.OptionalOptions` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.ParseJsonOptions` -> `none`: The old parse-json options type was removed; configure `fromJsonString` and its underlying getter directly.

- `Schema.PatternSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.Positive` -> `Schema.Number.check(Schema.isGreaterThan(0))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.PositiveBigDecimalFromSelf` -> `Schema.BigDecimal.check(Schema.isGreaterThanBigDecimal(BigDecimal.fromNumber(0)))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.PositiveBigDecimalSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.PositiveBigInt` -> `Schema.BigIntFromString.check(Schema.isGreaterThanBigInt(0n))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.PositiveBigIntFromSelf` -> `Schema.BigInt.check(Schema.isGreaterThanBigInt(0n))`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.PropertyKey` -> `Schema.PropertyKey`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.PropertySignature` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.PropertySignature.AST` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.PropertySignature.All` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.PropertySignature.Any` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.PropertySignature.Token` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.PropertySignatureDeclaration` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.PropertySignatureTransformation` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.PropertySignatureTypeId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.ReadonlyMap` -> `Schema.toCodecJson(Schema.ReadonlyMap(key, value))`: Pass key and value positionally and use the derived JSON codec to preserve v3's entry-array encoding.

- `Schema.ReadonlyMap$` -> `Schema.$ReadonlyMap`: Use the renamed v4 constructor result interface.

- `Schema.ReadonlyMapFromRecord` -> `none`: No direct record-to-ReadonlyMap codec remains; compose `Record` and `ReadonlyMap` with an explicit `decodeTo` transformation.

- `Schema.ReadonlyMapFromSelf` -> `Schema.ReadonlyMap`: The self schema dropped the `FromSelf` suffix; pass key and value positionally.

- `Schema.ReadonlySet` -> `Schema.toCodecJson(Schema.ReadonlySet(value))`: Use the derived JSON codec to preserve v3's array-to-ReadonlySet behavior.

- `Schema.ReadonlySet$` -> `Schema.$ReadonlySet`: Use the renamed v4 constructor result interface.

- `Schema.ReadonlySetFromSelf` -> `Schema.ReadonlySet`: The self schema dropped the `FromSelf` suffix.

- `Schema.Record` -> `Schema.Record(key, value)`: Pass key and value as separate arguments.

- `Schema.Record$` -> `Schema.$Record`: Use the renamed v4 constructor result interface.

- `Schema.Redacted` -> `Schema.RedactedFromValue`: Use `RedactedFromValue` to wrap decoded raw values; v4 `Redacted` is the self schema.

- `Schema.RedactedFromSelf` -> `Schema.Redacted`: The self schema dropped the `FromSelf` suffix.

- `Schema.RefineSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.RightEncoded` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.Schema` -> `Schema.Schema`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Schema.All` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Schema.Any` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Schema.AnyNoContext` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Schema.AsSchema` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Schema.Context` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Schema.Encoded` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Schema.ToAsserts` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Schema.Variance` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.SchemaClass` -> `Schema.Codec`: The concrete SchemaClass abstraction was removed; accept the appropriate v4 `Codec` or constraint type.

- `Schema.Serializable` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Serializable.All` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Serializable.Any` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Serializable.Context` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Serializable.Encoded` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Serializable.Type` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.SerializableWithResult` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.SerializableWithResult.All` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.SerializableWithResult.Any` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.SerializableWithResult.Context` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Set` -> `Schema.toCodecJson(Schema.ReadonlySet(value))`: Use the readonly Set schema and derive its JSON codec; mutable Set-specific schema types were removed.

- `Schema.Set$` -> `Schema.$ReadonlySet`: Use the renamed v4 constructor result interface.

- `Schema.SetFromSelf` -> `Schema.ReadonlySet`: Use the readonly Set self schema.

- `Schema.SimplifyMutable` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.SortedSet` -> `none`: The SortedSet schema was removed; migrate to `ReadonlySet` or declare a custom codec that applies the required ordering.

- `Schema.SortedSetFromSelf` -> `none`: The SortedSet self schema was removed; use `Schema.declare` if SortedSet values must remain in the model.

- `Schema.StartsWithSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.String` -> `Schema.String`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Struct` -> `Schema.Struct`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Struct.Constructor` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Struct.Context` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Struct.Encoded` -> `Schema.Struct.Encoded`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Struct.EncodedOptionalKeys` -> `Schema.Struct.EncodedOptionalKeys`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Struct.Field` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Struct.Key` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Struct.OptionalEncodedPropertySignature` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Struct.OptionalTypePropertySignature` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Struct.PropertySignatureWithDefault` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.Symbol` -> `none`: v4 `Symbol` is the self schema and has no built-in string-to-symbol codec; rebuild the conversion explicitly with `decodeTo`.

- `Schema.SymbolFromSelf` -> `Schema.Symbol`: The self schema dropped the `FromSelf` suffix.

- `Schema.TaggedClass` -> `Schema.TaggedClass`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.TaggedErrorClass` -> `Schema.TaggedError`: The exported helper interface was removed; use the class returned by Schema.TaggedError and infer its types.

- `Schema.TaggedRequest` -> `effect/rpc/Rpc.make`: The Schema request/serialization protocol was removed; migrate RPC requests to the v4 Rpc APIs.

- `Schema.TaggedRequest.All` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.TaggedRequest.Any` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.TaggedRequestClass` -> `effect/rpc/Rpc.make`: The Schema request/serialization protocol was removed; migrate RPC requests to the v4 Rpc APIs.

- `Schema.TaggedStruct` -> `Schema.TaggedStruct`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.TemplateLiteral` -> `Schema.TemplateLiteral(parts)`: Pass template literal parts as one array. Parts must not contain encodings, including inside unions and nested templates. Transformations whose decoded and encoded types are equal are also rejected. Use Schema.TemplateLiteralParser(parts) for transformed parts.

- `Schema.TemplateLiteralParser` -> `Schema.TemplateLiteralParser(parts)`: Pass template literal parts directly as one array. Transformed parts are supported, and their decoding and encoding services are required in the corresponding direction.

- `Schema.TimeZone` -> `Schema.TimeZoneFromString`: Use the string codec; v4 `TimeZone` is the self schema.

- `Schema.TimeZoneFromSelf` -> `Schema.TimeZone`: The self schema dropped the `FromSelf` suffix.

- `Schema.TimeZoneNamed` -> `Schema.TimeZoneNamedFromString`: Use the string codec; v4 `TimeZoneNamed` is the self schema.

- `Schema.TimeZoneNamedFromSelf` -> `Schema.TimeZoneNamed`: The self schema dropped the `FromSelf` suffix.

- `Schema.TimeZoneOffset` -> `Schema.toCodecJson(Schema.TimeZoneOffset)`: Use the derived JSON codec to preserve v3's encoded offset representation; v4 `TimeZoneOffset` is the self schema.

- `Schema.TimeZoneOffsetFromSelf` -> `Schema.TimeZoneOffset`: The self schema dropped the `FromSelf` suffix.

- `Schema.ToPropertySignature` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.Trim` -> `Schema.Trim`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Trimmed` -> `Schema.Trimmed`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.TrimmedSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.Tuple` -> `Schema.Tuple(elements)`: Pass tuple elements as one array.

- `Schema.Tuple2` -> `Schema.Tuple`: Use the array-based tuple constructor.

- `Schema.TupleType.ElementsEncoded` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.TupleType.ElementsType` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.TupleType.Encoded` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.TupleType.Type` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.TypeId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.TypeLiteral` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.TypeLiteral.Constructor` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.TypeLiteral.Encoded` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.TypeLiteral.Type` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.ULID` -> `Schema.String.check(Schema.isULID())`: Build the string schema with the ULID check.

- `Schema.ULIDSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.URL` -> `Schema.URLFromString`: Use the string-to-URL codec; v4 `URL` is the self schema.

- `Schema.URLFromSelf` -> `Schema.URL`: The self schema dropped the `FromSelf` suffix.

- `Schema.UUID` -> `Schema.String.check(Schema.isUUID())`: Build the string schema with the UUID check.

- `Schema.UUIDSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.Uint8` -> `Schema.Number.check(Schema.isInt(), Schema.isBetween({ minimum: 0, maximum: 255 }))`: Rebuild the unsigned-byte schema from integer and range checks.

- `Schema.Uint8Array` -> `Schema.toCodecJson(Schema.Uint8Array)`: Use the derived JSON codec to preserve v3's number-array encoding; v4 `Uint8Array` is the self schema.

- `Schema.Uint8ArrayFromSelf` -> `Schema.Uint8Array`: The self schema dropped the `FromSelf` suffix.

- `Schema.Uncapitalize` -> `Schema.String.pipe(Schema.decodeTo(Schema.String.check(Schema.isUncapitalized()), SchemaTransformation.uncapitalize()))`: Rebuild the uncapitalization transformation with `decodeTo`.

- `Schema.Uncapitalized` -> `Schema.String.check(Schema.isUncapitalized())`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.UncapitalizedSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.Undefined` -> `Schema.Undefined`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.UndefinedOr` -> `Schema.UndefinedOr`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Union` -> `Schema.Union(members)`: Pass union members as one array.

- `Schema.UniqueSymbolFromSelf` -> `Schema.UniqueSymbol`: Use the v4 unique-symbol schema constructor. JSON encoding is available only for globally registered symbols and uses the exact String(symbol) value; local symbols cannot be encoded as JSON.

- `Schema.Unknown` -> `Schema.Unknown`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.Uppercase` -> `Schema.String.pipe(Schema.decodeTo(Schema.String.check(Schema.isUppercased()), SchemaTransformation.toUpperCase()))`: Rebuild the uppercase transformation with `decodeTo`.

- `Schema.Uppercased` -> `Schema.String.check(Schema.isUppercased())`: Rebuild the removed convenience schema from the v4 base schema and check APIs.

- `Schema.UppercasedSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.ValidDateFromSelf` -> `Schema.Date`: Use the v4 Date self schema, which rejects invalid Date values.

- `Schema.ValidDateSchemaId` -> `none`: The v3 schema-id symbol was removed. Use the corresponding public v4 constructor/check instead of inspecting schema ids.

- `Schema.Void` -> `Schema.Void`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.WithResult` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.WithResult.All` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.WithResult.Any` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.WithResult.Context` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.WithResult.Failure` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.WithResult.FailureEncoded` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.WithResult.Success` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.WithResult.SuccessEncoded` -> `none`: The v3 helper/protocol type was removed by the v4 Schema model rewrite. Use the public v4 constructor and infer its result types instead.

- `Schema.annotations` -> `Schema.annotate`: Rename `annotations` to `annotate`.

- `Schema.asSchema` -> `Schema.revealCodec`: Use `revealCodec` to expose a schema's codec type.

- `Schema.asSerializable` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.asSerializableWithResult` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.asWithResult` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.attachPropertySignature` -> `Schema.tagDefaultOmit`: Map the struct fields and add `key: Schema.tagDefaultOmit(value)`; the old combinator was removed.

- `Schema.between` -> `Schema.isBetween`: Rename the predicate to `isBetween` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.betweenBigDecimal` -> `Schema.isBetweenBigDecimal`: Rename the predicate to `isBetweenBigDecimal` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.betweenBigInt` -> `Schema.isBetweenBigInt`: Rename the predicate to `isBetweenBigInt` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.betweenDate` -> `Schema.isBetweenDate`: Rename the predicate to `isBetweenDate` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.betweenDuration` -> `Schema.isBetween`: Rename the predicate to `isBetween` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.brand` -> `Schema.brand`: Pass one concrete string literal as the brand identifier. Widened strings, unions, open template literals, and symbol identifiers are rejected; apply brand repeatedly to compose brands. V4 branding is type-only and does not add runtime AST metadata.

- `Schema.capitalized` -> `Schema.isCapitalized`: Rename the string predicate to `isCapitalized` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.clamp` -> `Schema.decodeTo + SchemaGetter.transform(Number.clamp(...))`: Rebuild clamping as an explicit reversible transformation.

- `Schema.clampBigDecimal` -> `Schema.decodeTo + SchemaGetter.transform(BigDecimal.clamp(...))`: Rebuild BigDecimal clamping as an explicit reversible transformation.

- `Schema.clampBigInt` -> `Schema.decodeTo + SchemaGetter.transform(BigInt.clamp(...))`: Rebuild bigint clamping as an explicit reversible transformation.

- `Schema.clampDuration` -> `Schema.decodeTo + SchemaGetter.transform(Duration.clamp(...))`: Rebuild Duration clamping as an explicit reversible transformation.

- `Schema.declare` -> `Schema.declare`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.decode` -> `Schema.decodeEffect`: Rename the effectful decoder.

- `Schema.decodeEither` -> `Schema.decodeExit`: Rename the decoder returning an `Exit`.

- `Schema.decodeUnknown` -> `Schema.decodeUnknownEffect`: Rename the effectful unknown-input decoder.

- `Schema.decodeUnknownEither` -> `Schema.decodeUnknownExit`: Rename the unknown-input decoder returning an `Exit`.

- `Schema.decodeUnknownPromise` -> `Schema.decodeUnknownPromise`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.deserialize` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.deserializeExit` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.deserializeFailure` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.deserializeSuccess` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.element` -> `none`: The tuple element wrapper was removed; express elements directly in `Tuple([...])` or use `TupleWithRest` for rest elements.

- `Schema.encode` -> `Schema.encodeEffect`: Rename the effectful encoder.

- `Schema.encodeEither` -> `Schema.encodeExit`: Rename the encoder returning an `Exit`.

- `Schema.encodeUnknown` -> `Schema.encodeUnknownEffect`: Rename the effectful unknown-input encoder.

- `Schema.encodeUnknownEither` -> `Schema.encodeUnknownExit`: Rename the unknown-input encoder returning an `Exit`.

- `Schema.encodeUnknownPromise` -> `Schema.encodeUnknownPromise`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.encodedBoundSchema` -> `Schema.toEncoded`: Use the encoded side of the codec; service bounds are modeled by v4 codec service types.

- `Schema.encodedSchema` -> `Schema.toEncoded`: Rename the encoded-side projection.

- `Schema.endsWith` -> `Schema.isEndingWith`: Rename the string predicate to `isEndingWith` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.equivalence` -> `Schema.toEquivalence`: Rename the equivalence derivation utility.

- `Schema.exitSchema` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.extend` -> `schema.mapFields(Struct.assign(fields))`: Replace struct extension with `mapFields(Struct.assign(...))` or `Schema.fieldsAssign`; map union members explicitly.

- `Schema.failureSchema` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.filter` -> `Schema.check(Schema.makeFilter(predicate)) / Schema.refine(refinement)`: Use `check(makeFilter(...))` for predicates and `refine` for type refinements.

- `Schema.filterEffect` -> `Schema.decode({ decode: SchemaGetter.checkEffect(...), encode: SchemaGetter.passthrough() })`: Rebuild effectful validation as a decode step with `SchemaGetter.checkEffect`.

- `Schema.finite` -> `Schema.isFinite`: Rename the predicate to `isFinite` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.format` -> `SchemaRepresentation.toCodeDocument`: Build a representation with `SchemaRepresentation.toRepresentation`, `toMultiDocument`, then `toCodeDocument`.

- `Schema.fromBrand` -> `Schema.fromBrand`: Pass the constructor's single concrete string brand key as the first argument, followed by the constructor and then the schema. Apply fromBrand repeatedly for multiple brands; use Schema.Union for alternatives. Constructor checks remain at runtime, while the brand is type-only and does not add AST metadata.

- `Schema.fromKey` -> `Schema.encodeKeys`: Use `encodeKeys` to map decoded property names to encoded keys.

- `Schema.getNumberIndexedAccess` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.greaterThan` -> `Schema.isGreaterThan`: Rename the predicate to `isGreaterThan` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.greaterThanBigDecimal` -> `Schema.isGreaterThanBigDecimal`: Rename the predicate to `isGreaterThanBigDecimal` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.greaterThanBigInt` -> `Schema.isGreaterThanBigInt`: Rename the predicate to `isGreaterThanBigInt` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.greaterThanDate` -> `Schema.isGreaterThanDate`: Rename the predicate to `isGreaterThanDate` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.greaterThanDuration` -> `Schema.isGreaterThan`: Rename the predicate to `isGreaterThan` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.greaterThanOrEqualTo` -> `Schema.isGreaterThanOrEqualTo`: Rename the predicate to `isGreaterThanOrEqualTo` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.greaterThanOrEqualToBigDecimal` -> `Schema.isGreaterThanOrEqualToBigDecimal`: Rename the predicate to `isGreaterThanOrEqualToBigDecimal` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.greaterThanOrEqualToBigInt` -> `Schema.isGreaterThanOrEqualToBigInt`: Rename the predicate to `isGreaterThanOrEqualToBigInt` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.greaterThanOrEqualToDate` -> `Schema.isGreaterThanOrEqualToDate`: Rename the predicate to `isGreaterThanOrEqualToDate` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.greaterThanOrEqualToDuration` -> `Schema.isGreaterThanOrEqualTo`: Rename the predicate to `isGreaterThanOrEqualTo` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.head` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.headNonEmpty` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.headOrElse` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.includes` -> `Schema.isIncluding`: Rename the string predicate to `isIncluding` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.instanceOf` -> `Schema.instanceOf`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.int` -> `Schema.isInt`: Rename the predicate to `isInt` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.isPropertySignature` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.isSchema` -> `Schema.isSchema`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.itemsCount` -> `Schema.isBetweenLength`: Use the v4 collection-size check and apply it with `Schema.check` or a schema's `check` method.

- `Schema.keyof` -> `none`: Removed with the schema model rewrite; derive keys from struct fields or use `Schema.Literals` explicitly.

- `Schema.length` -> `Schema.isBetweenLength`: Use `isBetweenLength` with equal minimum and maximum values for an exact string length.

- `Schema.lessThan` -> `Schema.isLessThan`: Rename the predicate to `isLessThan` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.lessThanBigDecimal` -> `Schema.isLessThanBigDecimal`: Rename the predicate to `isLessThanBigDecimal` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.lessThanBigInt` -> `Schema.isLessThanBigInt`: Rename the predicate to `isLessThanBigInt` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.lessThanDate` -> `Schema.isLessThanDate`: Rename the predicate to `isLessThanDate` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.lessThanDuration` -> `Schema.isLessThan`: Rename the predicate to `isLessThan` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.lessThanOrEqualTo` -> `Schema.isLessThanOrEqualTo`: Rename the predicate to `isLessThanOrEqualTo` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.lessThanOrEqualToBigDecimal` -> `Schema.isLessThanOrEqualToBigDecimal`: Rename the predicate to `isLessThanOrEqualToBigDecimal` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.lessThanOrEqualToBigInt` -> `Schema.isLessThanOrEqualToBigInt`: Rename the predicate to `isLessThanOrEqualToBigInt` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.lessThanOrEqualToDate` -> `Schema.isLessThanOrEqualToDate`: Rename the predicate to `isLessThanOrEqualToDate` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.lessThanOrEqualToDuration` -> `Schema.isLessThanOrEqualTo`: Rename the predicate to `isLessThanOrEqualTo` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.lowercased` -> `Schema.isLowercased`: Rename the string predicate to `isLowercased` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.make` -> `Schema.make`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.makePropertySignature` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.maxItems` -> `Schema.isMaxLength`: Use the v4 collection-size check and apply it with `Schema.check` or a schema's `check` method.

- `Schema.maxLength` -> `Schema.isMaxLength`: Rename the string predicate to `isMaxLength` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.minItems` -> `Schema.isMinLength`: Use the v4 collection-size check and apply it with `Schema.check` or a schema's `check` method.

- `Schema.minLength` -> `Schema.isMinLength`: Rename the string predicate to `isMinLength` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.multipleOf` -> `Schema.isMultipleOf`: Rename the predicate to `isMultipleOf` and apply it with `Schema.check` or a schema's `check` method. The divisor must be finite and nonzero or construction throws RangeError; negative divisors are normalized to their absolute value.

- `Schema.mutable` -> `Schema.mutable`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.negative` -> `Schema.isLessThan(0)`: Use `isLessThan(0)` as a v4 check.

- `Schema.negativeBigDecimal` -> `Schema.isLessThanBigDecimal(BigDecimal.fromNumber(0))`: Use `isLessThanBigDecimal` as a v4 check.

- `Schema.negativeBigInt` -> `Schema.isLessThanBigInt(0n)`: Use `isLessThanBigInt(0n)` as a v4 check.

- `Schema.nonEmptyString` -> `Schema.isNonEmpty`: Rename the string predicate to `isNonEmpty` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.nonNaN` -> `Schema.makeFilter((n) => !Number.isNaN(n))`: Use an explicit filter because v4 has no dedicated non-NaN check.

- `Schema.nonNegative` -> `Schema.isGreaterThanOrEqualTo(0)`: Use `isGreaterThanOrEqualTo(0)` as a v4 check.

- `Schema.nonNegativeBigDecimal` -> `Schema.isGreaterThanOrEqualToBigDecimal(BigDecimal.fromNumber(0))`: Use `isGreaterThanOrEqualToBigDecimal` as a v4 check.

- `Schema.nonNegativeBigInt` -> `Schema.isGreaterThanOrEqualToBigInt(0n)`: Use `isGreaterThanOrEqualToBigInt(0n)` as a v4 check.

- `Schema.nonPositive` -> `Schema.isLessThanOrEqualTo(0)`: Use `isLessThanOrEqualTo(0)` as a v4 check.

- `Schema.nonPositiveBigDecimal` -> `Schema.isLessThanOrEqualToBigDecimal(BigDecimal.fromNumber(0))`: Use `isLessThanOrEqualToBigDecimal` as a v4 check.

- `Schema.nonPositiveBigInt` -> `Schema.isLessThanOrEqualToBigInt(0n)`: Use `isLessThanOrEqualToBigInt(0n)` as a v4 check.

- `Schema.omit` -> `schema.mapFields(Struct.omit([keys]))`: Use `mapFields` with `Struct.omit`; pass keys as an array.

- `Schema.optional` -> `Schema.optional`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.optionalElement` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.optionalToOptional` -> `Schema.decodeTo + SchemaGetter.transformOptional`: Rebuild optional-field transformations with `decodeTo` and `SchemaGetter.transformOptional`.

- `Schema.optionalToRequired` -> `Schema.decodeTo + SchemaGetter.transformOptional`: Rebuild optional-field transformations with `decodeTo` and `SchemaGetter.transformOptional`.

- `Schema.optionalWith` -> `Schema.optional / Schema.optionalKey / Schema.withDecodingDefaultType`: Choose `optional` or `optionalKey`; use the decoding-default helpers and an explicit nullable transformation as required by the old options.

- `Schema.parseJson` -> `Schema.fromJsonString(Schema.Unknown) / Schema.fromJsonString(schema)`: Use `fromJsonString(Schema.Unknown)` without an inner schema or `fromJsonString(schema)` with one.

- `Schema.parseNumber` -> `Schema.NumberFromString`: Use the built-in string-to-number codec. Unlike v3 `parseNumber`, invalid strings decode to `NaN`, while blank strings decode to `0`. `FiniteFromString` rejects non-finite results but also the `"NaN"` and infinity spellings accepted by v3; exact compatibility requires a custom codec.

- `Schema.partial` -> `schema.mapFields(Struct.map(Schema.optional))`: Map struct fields with `Schema.optional`.

- `Schema.partialWith` -> `schema.mapFields(Struct.map(Schema.optionalKey))`: For `{ exact: true }`, map struct fields with `Schema.optionalKey`; choose field helpers explicitly for other options.

- `Schema.pattern` -> `Schema.isPattern`: Rename the string predicate to `isPattern` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.pick` -> `schema.mapFields(Struct.pick([keys]))`: Use `mapFields` with `Struct.pick`; pass keys as an array.

- `Schema.pickLiteral` -> `Schema.Literals(values).pick(selected)`: Build a `Literals` schema from an array and call its `pick` method.

- `Schema.pluck` -> `none`: No direct replacement remains; pick the field then use `decodeTo` with `SchemaGetter.transform` to map between the field and enclosing object.

- `Schema.positive` -> `Schema.isGreaterThan(0)`: Use `isGreaterThan(0)` as a v4 check.

- `Schema.positiveBigDecimal` -> `Schema.isGreaterThanBigDecimal(BigDecimal.fromNumber(0))`: Use `isGreaterThanBigDecimal` as a v4 check.

- `Schema.positiveBigInt` -> `Schema.isGreaterThanBigInt(0n)`: Use `isGreaterThanBigInt(0n)` as a v4 check.

- `Schema.propertySignature` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.refine` -> `Schema.refine`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.rename` -> `Schema.encodeKeys`: Use `encodeKeys` for encoded-key renaming.

- `Schema.requiredToOptional` -> `Schema.decodeTo + SchemaGetter.transformOptional`: Rebuild optional-field transformations with `decodeTo` and `SchemaGetter.transformOptional`.

- `Schema.serializableSchema` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.serialize` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.serializeExit` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.serializeFailure` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.serializeSuccess` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.split` -> `Schema.String.pipe(Schema.decodeTo(Schema.Array(Schema.String), SchemaTransformation.transform(...)))`: Rebuild splitting as an explicit reversible string/array transformation.

- `Schema.standardSchemaV1` -> `Schema.toStandardSchemaV1`: Rename the Standard Schema adapter.

- `Schema.startsWith` -> `Schema.isStartingWith`: Rename the string predicate to `isStartingWith` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.successSchema` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.suspend` -> `Schema.suspend`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.symbolSerializable` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.symbolWithResult` -> `none`: Compared the v3 declaration with v4 Schema and the schema migration guide; no direct public replacement remains. Rebuild the behavior from public v4 codecs/getters where still required.

- `Schema.tag` -> `Schema.tag`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.transform` -> `schema.pipe(Schema.decodeTo(target, SchemaTransformation.transform({ decode, encode })))`: Replace the constructor with `decodeTo` and a `SchemaTransformation`.

- `Schema.transformLiteral` -> `Schema.Literal(from).transform(to)`: Use the literal schema's `transform` method.

- `Schema.transformLiterals` -> `Schema.Literals(fromValues).transform(toValues)`: Split the pairs into parallel arrays and use `Literals(...).transform(...)`.

- `Schema.transformOrFail` -> `schema.pipe(Schema.decodeTo(target, { decode: SchemaGetter.transformEffect(...), encode: ... }))`: Replace the constructor with `decodeTo` and effectful `SchemaGetter` transformations.

- `Schema.trimmed` -> `Schema.isTrimmed`: Rename the string predicate to `isTrimmed` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.typeSchema` -> `Schema.toType`: Rename the type-side projection.

- `Schema.uncapitalized` -> `Schema.isUncapitalized`: Rename the string predicate to `isUncapitalized` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.uppercased` -> `Schema.isUppercased`: Rename the string predicate to `isUppercased` and apply it with `Schema.check` or a schema's `check` method.

- `Schema.validDate` -> `Schema.Date`: Use the v4 Date self schema, which rejects invalid Date values.

- `Schema.validate` -> `Schema.decodeEffect(Schema.toType(schema))`: Validation was removed; decode through the schema's type side.

- `Schema.validateEither` -> `Schema.decodeExit(Schema.toType(schema))`: Validation was removed; decode through the schema's type side.

- `Schema.validatePromise` -> `Schema.decodePromise(Schema.toType(schema))`: Validation was removed; decode through the schema's type side.

- `Schema.withConstructorDefault` -> `Schema.withConstructorDefault`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.withDecodingDefault` -> `Schema.withDecodingDefault`: The API remains public in v4, but its type/value declaration was consolidated; use the v4 declaration and update inferred types/signature as needed.

- `Schema.withDefaults` -> `none`: Removed; choose `withConstructorDefault` and decoding-default helpers explicitly for each side.
