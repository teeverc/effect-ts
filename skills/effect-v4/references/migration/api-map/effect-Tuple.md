# v3 → v4 API map: effect/Tuple

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Tuple`

- `Tuple.TupleTypeLambda` -> `none`: Removed with tuple Bicovariant support; use the concrete tuple type or a local HKT TypeLambda.

- `Tuple.at` -> `Tuple.get`: Renamed for indexed access; v4 constrains the index to a valid tuple position.

- `Tuple.getEquivalence` -> `Tuple.makeEquivalence`: Pass equivalences as one array instead of variadic arguments.

- `Tuple.getFirst` -> `Tuple.get(0)`: Use Tuple.get(self, 0), or Tuple.get(0) in a pipe.

- `Tuple.getOrder` -> `Tuple.makeOrder`: Pass orders as one array instead of variadic arguments.

- `Tuple.getSecond` -> `Tuple.get(1)`: Use Tuple.get(self, 1), or Tuple.get(1) in a pipe.

- `Tuple.mapBoth` -> `Tuple.evolve`: Use Tuple.evolve(self, [options.onFirst, options.onSecond]).

- `Tuple.mapFirst` -> `Tuple.evolve`: Use Tuple.evolve(self, [f]); unspecified positions are preserved.

- `Tuple.mapSecond` -> `Tuple.evolve`: Use Tuple.evolve(self, [undefined, f]); undefined preserves the first position.

- `Tuple.swap` -> `Tuple.renameIndices`: Swap a pair with Tuple.renameIndices(self, ["1", "0"]).
