# v3 → v4 API map: effect/BigDecimal

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/BigDecimal`

- `BigDecimal.BigDecimal` -> `BigDecimal.BigDecimal`: The model interface remains, but its brand key is now internal.

- `BigDecimal.TypeId` -> `none`: The brand key is internal in v4; use BigDecimal.isBigDecimal for runtime narrowing.

- `BigDecimal.greaterThan` -> `BigDecimal.isGreaterThan`: Renamed with the v4 is-prefix.

- `BigDecimal.greaterThanOrEqualTo` -> `BigDecimal.isGreaterThanOrEqualTo`: Renamed with the v4 is-prefix.

- `BigDecimal.lessThan` -> `BigDecimal.isLessThan`: Renamed with the v4 is-prefix.

- `BigDecimal.lessThanOrEqualTo` -> `BigDecimal.isLessThanOrEqualTo`: Renamed with the v4 is-prefix.

- `BigDecimal.safeFromNumber` -> `BigDecimal.fromNumber`: Use the safe v4 constructor, which still returns Option.

- `BigDecimal.unsafeDivide` -> `BigDecimal.divideUnsafe`: Renamed to use the v4 Unsafe suffix convention.

- `BigDecimal.unsafeFromNumber` -> `BigDecimal.fromNumberUnsafe`: Renamed to use the v4 Unsafe suffix convention.

- `BigDecimal.unsafeFromString` -> `BigDecimal.fromStringUnsafe`: Renamed to use the v4 Unsafe suffix convention.

- `BigDecimal.unsafeRemainder` -> `BigDecimal.remainderUnsafe`: Renamed to use the v4 Unsafe suffix convention.

- `BigDecimal.unsafeToNumber` -> `BigDecimal.toNumberUnsafe`: Renamed to use the v4 Unsafe suffix convention.
