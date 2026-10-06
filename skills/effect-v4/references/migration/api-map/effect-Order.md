# v3 → v4 API map: effect/Order

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Order`

- `Order.Order` -> `Order.Order`: The callable type is retained; its return type remains the -1 | 0 | 1 Ordering union.

- `Order.all` -> `Order.Tuple([...collection])`: Materialize the comparator iterable for Tuple. V4 evaluates the configured tuple instead of stopping at the shorter input; use Order.make for intentional v3 prefix semantics.

- `Order.array` -> `Order.Array`: Capitalized constructor name; lexicographic array ordering and the length tie-break are unchanged.

- `Order.between` -> `Order.isBetween`: Renamed with the v4 is-prefix; inclusive bounds and call forms are unchanged.

- `Order.bigint` -> `Order.BigInt`: Capitalized instance name; bigint ordering is unchanged.

- `Order.boolean` -> `Order.Boolean`: Capitalized instance name; false remains ordered before true.

- `Order.combineAll` -> `Order.combineAll`: Retained with the same left-to-right tie-breaking and empty-iterable result.

- `Order.combineMany` -> `Order.combine(self, Order.combineAll(collection))`: Compose combine with combineAll; the dedicated dual combineMany helper was removed.

- `Order.empty` -> `Order.alwaysEqual`: Renamed constructor; call as Order.alwaysEqual\<A\>() to produce an order that always returns zero.

- `Order.greaterThan` -> `Order.isGreaterThan`: Renamed with the v4 is-prefix; curried and uncurried comparisons are retained.

- `Order.greaterThanOrEqualTo` -> `Order.isGreaterThanOrEqualTo`: Renamed with the v4 is-prefix; curried and uncurried comparisons are retained.

- `Order.lessThan` -> `Order.isLessThan`: Renamed with the v4 is-prefix; curried and uncurried comparisons are retained.

- `Order.lessThanOrEqualTo` -> `Order.isLessThanOrEqualTo`: Renamed with the v4 is-prefix; curried and uncurried comparisons are retained.

- `Order.make` -> `Order.make`: Retained with the same comparator contract and reference-equality fast path.

- `Order.number` -> `Order.Number`: Capitalized instance name. V4 orders NaN below non-NaN values and all NaNs equally; use a custom Order.make to preserve v3 edge behavior.

- `Order.product` -> `Order.Tuple([self, that])`: Replace the dual two-order helper with the single-array Tuple constructor.

- `Order.productMany` -> `Order.Tuple([self, ...collection])`: Materialize the order iterable in one Tuple call; v4 evaluates every configured comparator for short inputs.

- `Order.reverse` -> `Order.flip`: Direct rename; the replacement reverses comparison by swapping the operands.

- `Order.string` -> `Order.String`: Capitalized instance name; case-sensitive JavaScript lexicographic ordering is unchanged.

- `Order.struct` -> `Order.Struct`: Capitalized constructor name; field-order tie-breaking is unchanged.

- `Order.tuple` -> `Order.Tuple([orderA, orderB, ...])`: Capitalized constructor now takes one comparator array instead of rest arguments and evaluates every configured position.
