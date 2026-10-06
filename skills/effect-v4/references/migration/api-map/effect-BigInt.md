# v3 → v4 API map: effect/BigInt

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/BigInt`

- `BigInt.fromNumber` -> `BigInt.fromNumber`: Unchanged; it returns Option for safe conversion.

- `BigInt.greaterThan` -> `BigInt.isGreaterThan`: Renamed with the v4 is-prefix.

- `BigInt.greaterThanOrEqualTo` -> `BigInt.isGreaterThanOrEqualTo`: Renamed with the v4 is-prefix.

- `BigInt.lessThan` -> `BigInt.isLessThan`: Renamed with the v4 is-prefix.

- `BigInt.lessThanOrEqualTo` -> `BigInt.isLessThanOrEqualTo`: Renamed with the v4 is-prefix.

- `BigInt.unsafeDivide` -> `BigInt.divideUnsafe`: Renamed to use the v4 Unsafe suffix convention.

- `BigInt.unsafeSqrt` -> `BigInt.sqrtUnsafe`: Renamed to use the v4 Unsafe suffix convention.
