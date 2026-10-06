# v3 → v4 API map: effect/Predicate

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Predicate`

- `Predicate.Predicate` -> `Predicate.Predicate`: The callable interface is retained. Predicate.Any now uses any rather than never, which can affect generic inference.

- `Predicate.Refinement` -> `Predicate.Refinement`: The refinement interface and its In, Out, and Any namespace types are retained.

- `Predicate.all` -> `Predicate.Tuple(Array.from(collection))`: Use positional Tuple, materializing an Iterable when needed. V4 checks every configured position instead of accepting missing input values.

- `Predicate.every` -> `Predicate.every`: Retained with the same AND semantics, short-circuiting, and true result for an empty collection.

- `Predicate.isBigInt` -> `Predicate.isBigInt`: Retained with the same bigint refinement.

- `Predicate.isBoolean` -> `Predicate.isBoolean`: Retained with the same boolean refinement.

- `Predicate.isDate` -> `Predicate.isDate`: Retained with the same instanceof Date check.

- `Predicate.isError` -> `Predicate.isError`: Retained with the same instanceof Error check.

- `Predicate.isFunction` -> `Predicate.isFunction`: Retained with the same function refinement.

- `Predicate.isIterable` -> `Predicate.isIterable`: Retained; strings and values exposing Symbol.iterator are still accepted.

- `Predicate.isMap` -> `Predicate.isMap`: Retained with the same instanceof Map check.

- `Predicate.isNever` -> `Predicate.isNever`: Retained as the always-false refinement.

- `Predicate.isNotNull` -> `Predicate.isNotNull`: Retained; undefined still passes while null is excluded.

- `Predicate.isNotNullable` -> `Predicate.isNotNullish`: Renamed to use nullish terminology; it still excludes null and undefined.

- `Predicate.isNotUndefined` -> `Predicate.isNotUndefined`: Retained; null still passes while undefined is excluded.

- `Predicate.isNull` -> `Predicate.isNull`: Retained with the same strict null refinement.

- `Predicate.isNullable` -> `Predicate.isNullish`: Renamed to use nullish terminology. The guard now narrows with A & (null | undefined), including unknown inputs correctly.

- `Predicate.isNumber` -> `Predicate.isNumber`: Retained; NaN and infinite numbers still pass.

- `Predicate.isObject` -> `Predicate.isObjectKeyword`: Use isObjectKeyword to preserve v3 behavior accepting arrays and functions. V4 isObject has the former record-like semantics instead.

- `Predicate.isPromise` -> `Predicate.isPromise`: Retained as the structural check for callable then and catch properties.

- `Predicate.isPromiseLike` -> `Predicate.isPromiseLike`: Retained as the structural check for a callable then property.

- `Predicate.isReadonlyRecord` -> `Predicate.isReadonlyObject`: Renamed; runtime behavior is unchanged and the index-key type now explicitly includes numbers.

- `Predicate.isRecord` -> `Predicate.isObject`: Renamed; it still accepts non-null, non-array objects and now narrows with PropertyKey indexes.

- `Predicate.isRegExp` -> `Predicate.isRegExp`: Retained with the same instanceof RegExp check.

- `Predicate.isSet` -> `Predicate.isSet`: Retained with the same instanceof Set check.

- `Predicate.isString` -> `Predicate.isString`: Retained with the same primitive string refinement.

- `Predicate.isSymbol` -> `Predicate.isSymbol`: Retained with the same symbol refinement.

- `Predicate.isTruthy` -> `Predicate.isTruthy`: Retained as a plain boolean predicate using JavaScript truthiness.

- `Predicate.isUint8Array` -> `Predicate.isUint8Array`: Retained with the same instanceof Uint8Array check.

- `Predicate.isUndefined` -> `Predicate.isUndefined`: Retained with the same strict undefined refinement.

- `Predicate.isUnknown` -> `Predicate.isUnknown`: Retained as the always-true refinement.

- `Predicate.not` -> `Predicate.not`: Retained with the same boolean negation; refinements still become plain predicates.

- `Predicate.product` -> `Predicate.Tuple([self, that])`: Replace the two-position product helper with the Tuple constructor.

- `Predicate.productMany` -> `Predicate.Tuple([self, ...Array.from(collection)])`: Materialize the predicate iterable in one Tuple call; v4 checks missing tail positions as undefined.

- `Predicate.some` -> `Predicate.some`: Retained with the same OR semantics, short-circuiting, and false result for an empty collection.

- `Predicate.struct` -> `Predicate.Struct`: Capitalized constructor name; field checks and refinement-aware typing are retained.

- `Predicate.tuple` -> `Predicate.Tuple([p1, p2, ...])`: Capitalized constructor now takes one predicate array instead of rest arguments and checks missing positions as undefined.
