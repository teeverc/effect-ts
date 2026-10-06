# v3 → v4 API map: @effect/typeclass (17 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/typeclass/Bounded`

- `Bounded.Bounded` -> `none`: V4 removed Bounded dictionaries. Keep the Order and minimum/maximum bounds as separate application values.

- `Bounded.BoundedTypeLambda` -> `none`: V4 removed the @effect/typeclass higher-kinded Bounded instance machinery.

- `Bounded.between` -> `Order.isBetween(B.compare)`: Use the v4 Order predicate with { minimum: B.minBound, maximum: B.maxBound }; the Bounded dictionary itself was removed.

- `Bounded.clamp` -> `Order.clamp(B.compare)`: Use the v4 Order combinator with { minimum: B.minBound, maximum: B.maxBound }; the Bounded dictionary itself was removed.

- `Bounded.max` -> `Reducer.make(Combiner.max(B.compare).combine, B.minBound)`: V4 removed Bounded dictionaries. Build the maximum Reducer from the separately retained Order and minimum bound.

- `Bounded.min` -> `Reducer.make(Combiner.min(B.compare).combine, B.maxBound)`: V4 removed Bounded dictionaries. Build the minimum Reducer from the separately retained Order and maximum bound.

- `Bounded.reverse` -> `Order.flip(B.compare)`: Flip the Order and swap the separately stored minimum and maximum bounds; v4 has no bundled Bounded dictionary.

## `@effect/typeclass/Monoid`

- `Monoid.Monoid` -> `Reducer.Reducer`: Reducer replaces Monoid in v4; empty is renamed initialValue and combineAll remains available.

- `Monoid.array` -> `Array.makeReducerConcat`: Use the v4 array concatenation Reducer; Reducer replaces Monoid and names the identity initialValue.

- `Monoid.fromSemigroup` -> `Reducer.make(S.combine, empty)`: Construct a v4 Reducer from the replacement Combiner operation and identity value.

- `Monoid.max` -> `Reducer.make(Combiner.max(B.compare).combine, B.minBound)`: Reducer replaces Monoid. Build it from the v4 maximum Combiner and the bounded order's minimum value.

- `Monoid.min` -> `Reducer.make(Combiner.min(B.compare).combine, B.maxBound)`: Reducer replaces Monoid. Build it from the v4 minimum Combiner and the bounded order's maximum value.

- `Monoid.reverse` -> `Reducer.flip`: Use the v4 Reducer combinator; it preserves initialValue and reverses combine argument order.

- `Monoid.struct` -> `Struct.makeReducer`: Pass a record of v4 Reducers to derive a field-wise Reducer.

- `Monoid.tuple` -> `Tuple.makeReducer`: Pass one array of v4 Reducers instead of rest Monoid arguments.

## `@effect/typeclass/Semigroup`

- `Semigroup.Invariant` -> `none`: The @effect/typeclass package was removed in v4 with no generic typeclass layer replacement. Rewrite this abstraction against the concrete v4 data type and its module functions.

- `Semigroup.Product` -> `none`: The @effect/typeclass package was removed in v4 with no generic typeclass layer replacement. Rewrite this abstraction against the concrete v4 data type and its module functions.

- `Semigroup.SemiProduct` -> `none`: The @effect/typeclass package was removed in v4 with no generic typeclass layer replacement. Rewrite this abstraction against the concrete v4 data type and its module functions.

- `Semigroup.Semigroup` -> `Combiner.Combiner`: Combiner replaces Semigroup in v4 and retains the binary combine method; combineMany was removed.

- `Semigroup.SemigroupTypeLambda` -> `none`: The @effect/typeclass package was removed in v4 with no generic typeclass layer replacement. Rewrite this abstraction against the concrete v4 data type and its module functions.

- `Semigroup.array` -> `Array.makeReducerConcat`: The v4 concatenation Reducer is also a Combiner and replaces the array Semigroup.

- `Semigroup.constant` -> `Combiner.constant`: Combiner replaces Semigroup in v4.

- `Semigroup.first` -> `Combiner.first`: Combiner replaces Semigroup in v4.

- `Semigroup.imap` -> `Combiner.make`: V4 has no generic invariant instance; build a Combiner that maps both inputs with from, combines them, then maps the result with to.

- `Semigroup.intercalate` -> `Combiner.intercalate`: Combiner replaces Semigroup; v4 takes the separator first and then the Combiner.

- `Semigroup.last` -> `Combiner.last`: Combiner replaces Semigroup in v4.

- `Semigroup.make` -> `Combiner.make`: Combiner replaces Semigroup. V4 accepts only the binary combine function and has no combineMany override.

- `Semigroup.max` -> `Combiner.max`: Combiner replaces Semigroup; pass the same Order to retain last-maximum tie behavior.

- `Semigroup.min` -> `Combiner.min`: Combiner replaces Semigroup; pass the same Order to retain last-minimum tie behavior.

- `Semigroup.reverse` -> `Combiner.flip`: Use the v4 Combiner combinator to reverse combine argument order.

- `Semigroup.struct` -> `Struct.makeCombiner`: Pass a record of v4 Combiners to derive a field-wise Combiner.

- `Semigroup.tuple` -> `Tuple.makeCombiner`: Pass one array of v4 Combiners instead of rest Semigroup arguments.

## `@effect/typeclass/data/Array`

- `Array.Applicative` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.Chainable` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.Covariant` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.Filterable` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.FlatMap` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.Foldable` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.Invariant` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.Monad` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.Of` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.Pointed` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.Product` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.SemiApplicative` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.SemiProduct` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.Traversable` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.TraversableFilterable` -> `effect/Array`: The @effect/typeclass package and its Array instance dictionaries were removed in v4. Use the concrete effect/Array operations directly.

- `Array.getMonoid` -> `Array.makeReducerConcat`: Use the v4 concatenation Reducer; Reducer replaces Monoid.

- `Array.getSemigroup` -> `Array.makeReducerConcat`: The v4 concatenation Reducer is also a Combiner and replaces this Semigroup.

## `@effect/typeclass/data/BigInt`

- `BigInt.MonoidMultiply` -> `BigInt.ReducerMultiply`: Renamed and moved to the concrete v4 BigInt module.

- `BigInt.MonoidSum` -> `BigInt.ReducerSum`: Renamed and moved to the concrete v4 BigInt module.

- `BigInt.SemigroupMax` -> `BigInt.CombinerMax`: Renamed and moved to the concrete v4 BigInt module.

- `BigInt.SemigroupMin` -> `BigInt.CombinerMin`: Renamed and moved to the concrete v4 BigInt module.

- `BigInt.SemigroupMultiply` -> `BigInt.ReducerMultiply`: The v4 Reducer is also a Combiner and preserves multiplication combine semantics.

- `BigInt.SemigroupSum` -> `BigInt.ReducerSum`: The v4 Reducer is also a Combiner and preserves addition combine semantics.

## `@effect/typeclass/data/Boolean`

- `Boolean.MonoidEqv` -> `Reducer.make(Boolean.eqv, true)`: Rebuild the removed instance with the v4 boolean operation and its identity.

- `Boolean.MonoidEvery` -> `Boolean.ReducerAnd`: Renamed and moved to the concrete v4 Boolean module.

- `Boolean.MonoidSome` -> `Boolean.ReducerOr`: Renamed and moved to the concrete v4 Boolean module.

- `Boolean.MonoidXor` -> `Reducer.make(Boolean.xor, false)`: Rebuild the removed instance with the v4 boolean operation and its identity.

- `Boolean.SemigroupEqv` -> `Combiner.make(Boolean.eqv)`: Rebuild the removed instance as a v4 Combiner.

- `Boolean.SemigroupEvery` -> `Boolean.ReducerAnd`: The v4 Reducer is also a Combiner and preserves logical-AND combine semantics.

- `Boolean.SemigroupSome` -> `Boolean.ReducerOr`: The v4 Reducer is also a Combiner and preserves logical-OR combine semantics.

- `Boolean.SemigroupXor` -> `Combiner.make(Boolean.xor)`: Rebuild the removed instance as a v4 Combiner.

## `@effect/typeclass/data/Duration`

- `Duration.Bounded` -> `none`: V4 has no Bounded dictionary; use Duration.Order with Duration.zero and Duration.infinity as separate bounds.

- `Duration.MonoidMax` -> `Reducer.make(Duration.max, Duration.zero)`: Rebuild the removed maximum Monoid as a v4 Reducer with the same identity.

- `Duration.MonoidMin` -> `Reducer.make(Duration.min, Duration.infinity)`: Rebuild the removed minimum Monoid as a v4 Reducer with the same identity.

- `Duration.MonoidSum` -> `Duration.ReducerSum`: Renamed and moved to the concrete v4 Duration module.

- `Duration.SemigroupMax` -> `Duration.CombinerMax`: Renamed and moved to the concrete v4 Duration module.

- `Duration.SemigroupMin` -> `Duration.CombinerMin`: Renamed and moved to the concrete v4 Duration module.

- `Duration.SemigroupSum` -> `Duration.ReducerSum`: The v4 Reducer is also a Combiner and preserves Duration.sum combine semantics.

## `@effect/typeclass/data/Effect`

- `Effect.Chainable` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.ConcurrencyOptions` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.Covariant` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.FlatMap` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.Invariant` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.Monad` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.Of` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.Pointed` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.getApplicative` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.getProduct` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.getSemiApplicative` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

- `Effect.getSemiProduct` -> `effect/Effect`: The @effect/typeclass package and its Effect instance dictionaries were removed in v4. Use the concrete effect/Effect operations directly.

## `@effect/typeclass/data/Either`

- `Either.Applicative` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.Bicovariant` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.Chainable` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.Covariant` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.FlatMap` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.Foldable` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.Invariant` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.Monad` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.Of` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.Pointed` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.Product` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.SemiAlternative` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.SemiApplicative` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.SemiCoproduct` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.SemiProduct` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

- `Either.Traversable` -> `effect/Either`: The @effect/typeclass package and its Either instance dictionaries were removed in v4. Use the concrete effect/Either operations directly.

## `@effect/typeclass/data/Micro`

- `Micro.Chainable` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.ConcurrencyOptions` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.Covariant` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.FlatMap` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.Invariant` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.Monad` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.Of` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.Pointed` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.getApplicative` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.getProduct` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.getSemiApplicative` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

- `Micro.getSemiProduct` -> `effect/Micro`: The @effect/typeclass package and its Micro instance dictionaries were removed in v4. Use the concrete effect/Micro operations directly.

## `@effect/typeclass/data/Number`

- `Number.Bounded` -> `none`: V4 has no Bounded dictionary; use Number.Order with -Infinity and Infinity as separate bounds.

- `Number.MonoidMax` -> `Number.ReducerMax`: Renamed and moved to the concrete v4 Number module.

- `Number.MonoidMin` -> `Number.ReducerMin`: Renamed and moved to the concrete v4 Number module.

- `Number.MonoidMultiply` -> `Number.ReducerMultiply`: Renamed and moved to the concrete v4 Number module.

- `Number.MonoidSum` -> `Number.ReducerSum`: Renamed and moved to the concrete v4 Number module.

- `Number.SemigroupMax` -> `Number.ReducerMax`: The v4 Reducer is also a Combiner and preserves maximum combine semantics.

- `Number.SemigroupMin` -> `Number.ReducerMin`: The v4 Reducer is also a Combiner and preserves minimum combine semantics.

- `Number.SemigroupMultiply` -> `Number.ReducerMultiply`: The v4 Reducer is also a Combiner and preserves multiplication combine semantics.

- `Number.SemigroupSum` -> `Number.ReducerSum`: The v4 Reducer is also a Combiner and preserves addition combine semantics.

## `@effect/typeclass/data/Option`

- `Option.Alternative` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Applicative` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Chainable` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Coproduct` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Covariant` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Filterable` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.FlatMap` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Foldable` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Invariant` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Monad` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Of` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Pointed` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Product` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.SemiAlternative` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.SemiApplicative` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.SemiCoproduct` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.SemiProduct` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.Traversable` -> `effect/Option`: The @effect/typeclass package and its Option instance dictionaries were removed in v4. Use the concrete effect/Option operations directly.

- `Option.getOptionalMonoid` -> `Option.makeReducer`: Pass the replacement Combiner; the v4 Reducer uses None as initialValue and combines two Some values.

## `@effect/typeclass/data/Ordering`

- `Ordering.Monoid` -> `Ordering.Reducer`: Renamed and moved to the concrete v4 Ordering module.

- `Ordering.Semigroup` -> `Ordering.Reducer`: The v4 Reducer is also a Combiner and preserves Ordering combination semantics.

## `@effect/typeclass/data/Predicate`

- `Predicate.Contravariant` -> `effect/Predicate`: The @effect/typeclass package and its Predicate instance dictionaries were removed in v4. Use the concrete effect/Predicate operations directly.

- `Predicate.Invariant` -> `effect/Predicate`: The @effect/typeclass package and its Predicate instance dictionaries were removed in v4. Use the concrete effect/Predicate operations directly.

- `Predicate.Of` -> `effect/Predicate`: The @effect/typeclass package and its Predicate instance dictionaries were removed in v4. Use the concrete effect/Predicate operations directly.

- `Predicate.Product` -> `effect/Predicate`: The @effect/typeclass package and its Predicate instance dictionaries were removed in v4. Use the concrete effect/Predicate operations directly.

- `Predicate.SemiProduct` -> `effect/Predicate`: The @effect/typeclass package and its Predicate instance dictionaries were removed in v4. Use the concrete effect/Predicate operations directly.

- `Predicate.getMonoidEqv` -> `Reducer.make(Predicate.eqv, Predicate.isUnknown)`: Rebuild the removed predicate instance as a v4 Reducer with the always-true predicate as initialValue.

- `Predicate.getMonoidEvery` -> `Reducer.make(Predicate.and, Predicate.isUnknown)`: Rebuild the removed predicate instance as a v4 Reducer with the always-true predicate as initialValue.

- `Predicate.getMonoidSome` -> `Reducer.make(Predicate.or, Predicate.isNever)`: Rebuild the removed predicate instance as a v4 Reducer with the always-false predicate as initialValue.

- `Predicate.getMonoidXor` -> `Reducer.make(Predicate.xor, Predicate.isNever)`: Rebuild the removed predicate instance as a v4 Reducer with the always-false predicate as initialValue.

- `Predicate.getSemigroupEqv` -> `Combiner.make(Predicate.eqv)`: Rebuild the removed predicate instance as a v4 Combiner.

- `Predicate.getSemigroupEvery` -> `Combiner.make(Predicate.and)`: Rebuild the removed predicate instance as a v4 Combiner.

- `Predicate.getSemigroupSome` -> `Combiner.make(Predicate.or)`: Rebuild the removed predicate instance as a v4 Combiner.

- `Predicate.getSemigroupXor` -> `Combiner.make(Predicate.xor)`: Rebuild the removed predicate instance as a v4 Combiner.

## `@effect/typeclass/data/Record`

- `Record.Covariant` -> `effect/Record`: The @effect/typeclass package and its Record instance dictionaries were removed in v4. Use the concrete effect/Record operations directly.

- `Record.Filterable` -> `effect/Record`: The @effect/typeclass package and its Record instance dictionaries were removed in v4. Use the concrete effect/Record operations directly.

- `Record.Invariant` -> `effect/Record`: The @effect/typeclass package and its Record instance dictionaries were removed in v4. Use the concrete effect/Record operations directly.

- `Record.Traversable` -> `effect/Record`: The @effect/typeclass package and its Record instance dictionaries were removed in v4. Use the concrete effect/Record operations directly.

- `Record.TraversableFilterable` -> `effect/Record`: The @effect/typeclass package and its Record instance dictionaries were removed in v4. Use the concrete effect/Record operations directly.

- `Record.getCovariant` -> `effect/Record`: The @effect/typeclass package and its Record instance dictionaries were removed in v4. Use the concrete effect/Record operations directly.

- `Record.getFilterable` -> `effect/Record`: The @effect/typeclass package and its Record instance dictionaries were removed in v4. Use the concrete effect/Record operations directly.

- `Record.getInvariant` -> `effect/Record`: The @effect/typeclass package and its Record instance dictionaries were removed in v4. Use the concrete effect/Record operations directly.

- `Record.getMonoidUnion` -> `Record.makeReducerUnion`: Pass the replacement value Combiner; the v4 Reducer uses an empty record as initialValue.

- `Record.getSemigroupIntersection` -> `Record.makeReducerIntersection`: Pass the replacement value Combiner and use the returned Reducer's combine operation for pairwise intersection.

- `Record.getSemigroupUnion` -> `Record.makeReducerUnion`: The v4 Reducer is also a Combiner and preserves pairwise union semantics.

- `Record.getTraversable` -> `effect/Record`: The @effect/typeclass package and its Record instance dictionaries were removed in v4. Use the concrete effect/Record operations directly.

- `Record.getTraversableFilterable` -> `effect/Record`: The @effect/typeclass package and its Record instance dictionaries were removed in v4. Use the concrete effect/Record operations directly.

## `@effect/typeclass/data/String`

- `String.Monoid` -> `String.ReducerConcat`: Renamed and moved to the concrete v4 String module.

- `String.Semigroup` -> `String.ReducerConcat`: The v4 Reducer is also a Combiner and preserves string concatenation.

## `@effect/typeclass/data/Tuple`

- `Tuple.Bicovariant` -> `effect/Tuple`: The @effect/typeclass package and its Tuple instance dictionaries were removed in v4. Use the concrete effect/Tuple operations directly.
