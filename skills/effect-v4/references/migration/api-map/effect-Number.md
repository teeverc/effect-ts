# v3 → v4 API map: effect/Number

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Number`

- `Number.greaterThan` -> `Number.isGreaterThan`: Renamed with the v4 is-prefix.

- `Number.greaterThanOrEqualTo` -> `Number.isGreaterThanOrEqualTo`: Renamed with the v4 is-prefix.

- `Number.lessThan` -> `Number.isLessThan`: Renamed with the v4 is-prefix.

- `Number.lessThanOrEqualTo` -> `Number.isLessThanOrEqualTo`: Renamed with the v4 is-prefix.

- `Number.negate` -> `Number.multiply(-1)`: Use Number.multiply(n, -1), or Number.multiply(-1) as the equivalent unary function.

- `Number.unsafeDivide` -> `Number.divideUnsafe`: Renamed; v4 throws for zero whereas v3 raw division returned Infinity or NaN.
