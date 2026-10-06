# v3 → v4 API map: effect/Duration

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Duration`

- `Duration.Duration` -> `Duration.Duration`: The model remains and now also supports negative infinity; its type-id value is private.

- `Duration.DurationInput` -> `Duration.Input`: The input type was renamed and expanded with negative values and Temporal.Duration-like objects.

- `Duration.DurationValue` -> `Duration.DurationValue`: The tagged value remains and adds NegativeInfinity; its object fields are no longer readonly.

- `Duration.TypeId` -> `none`: The Duration type id is private in v4. Use Duration.isDuration to narrow unknown values.

- `Duration.decode` -> `Duration.fromInputUnsafe`: The throwing DurationInput decoder was renamed.

- `Duration.decodeUnknown` -> `Duration.fromInput`: The safe decoder was renamed and accepts Duration.Input, returning Option\<Duration\>.

- `Duration.formatIso` -> `none`: ISO 8601 duration formatting was removed from the v4 Duration module. The v4 source and migration guides expose no direct replacement; retain a local formatter when this wire format is required.

- `Duration.fromIso` -> `none`: ISO 8601 duration parsing was removed from the v4 Duration module. The v4 source and migration guides expose no direct replacement; use a dedicated ISO parser and pass the resulting parts to Duration.fromInput.

- `Duration.greaterThan` -> `Duration.isGreaterThan`: The comparison was renamed with the is prefix.

- `Duration.greaterThanOrEqualTo` -> `Duration.isGreaterThanOrEqualTo`: The comparison was renamed with the is prefix.

- `Duration.lessThan` -> `Duration.isLessThan`: The comparison was renamed with the is prefix.

- `Duration.lessThanOrEqualTo` -> `Duration.isLessThanOrEqualTo`: The comparison was renamed with the is prefix.

- `Duration.matchWith` -> `Duration.matchPair`: The two-duration matcher was renamed.

- `Duration.unsafeDivide` -> `Duration.divideUnsafe`: The unsafe prefix moved to the end of the division function name.

- `Duration.unsafeFormatIso` -> `none`: ISO 8601 duration formatting was removed from v4. The v4 Duration exports and migration guides contain no direct unsafe formatter; retain a local formatter if required.

- `Duration.unsafeToNanos` -> `Duration.toNanosUnsafe`: The unsafe prefix moved to the end of the nanosecond conversion name.
