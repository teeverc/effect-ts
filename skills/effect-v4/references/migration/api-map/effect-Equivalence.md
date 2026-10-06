# v3 → v4 API map: effect/Equivalence

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Equivalence`

- `Equivalence.Equivalence` -> `Equivalence.Equivalence`: The callable type is retained but is now a type alias, so declaration merging is no longer supported.

- `Equivalence.all` -> `Equivalence.Tuple([...collection])`: Materialize the comparator iterable for Tuple. Unlike v3 prefix comparison, v4 requires equal input lengths; use Equivalence.make for intentional prefix semantics.

- `Equivalence.array` -> `Equivalence.Array`: Capitalized constructor name; positional equal-length array comparison is unchanged.

- `Equivalence.bigint` -> `Equivalence.BigInt`: Capitalized instance name; strict bigint equality is unchanged.

- `Equivalence.boolean` -> `Equivalence.Boolean`: Capitalized instance name; strict boolean equality is unchanged.

- `Equivalence.combineMany` -> `Equivalence.combine(self, Equivalence.combineAll(collection))`: Compose combine with combineAll; the dedicated dual combineMany helper was removed.

- `Equivalence.number` -> `Equivalence.Number`: Capitalized instance name. V4 considers NaN equivalent to NaN; use Equivalence.strictEqual\<number\>() for exact v3 strict-equality behavior.

- `Equivalence.product` -> `Equivalence.Tuple([self, that])`: Replace the dual two-comparator helper with the single-array Tuple constructor.

- `Equivalence.productMany` -> `Equivalence.Tuple([self, ...collection])`: Materialize the comparator iterable in one Tuple call; v4 rejects unequal input lengths instead of using v3 prefix semantics.

- `Equivalence.strict` -> `Equivalence.strictEqual`: Renamed strict-equality constructor; call as Equivalence.strictEqual\<A\>().

- `Equivalence.string` -> `Equivalence.String`: Capitalized instance name; case-sensitive strict equality is unchanged.

- `Equivalence.struct` -> `Equivalence.Struct`: Capitalized constructor name. V4 also compares configured symbol and non-enumerable keys via Reflect.ownKeys.

- `Equivalence.symbol` -> `Equivalence.strictEqual<symbol>()`: There is no Symbol instance export; strictEqual preserves the v3 symbol comparison.

- `Equivalence.tuple` -> `Equivalence.Tuple([eqA, eqB, ...])`: Capitalized constructor now takes one comparator array instead of rest arguments and rejects unequal input lengths.
