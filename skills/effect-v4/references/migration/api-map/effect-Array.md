# v3 → v4 API map: effect/Array

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Array`

- `Array.ReadonlyArray` -> `Array.ReadonlyArray`: The namespace and its Infer, With, OrNonEmpty, AndNonEmpty, and Flatten utility types remain.

- `Array.filterMapWhile` -> `Array.takeWhileFilter`: Same map-until-first-miss behavior; change the callback from Option.some/none to Result.succeed/fail.

- `Array.flatMapNullable` -> `Array.flatMapNullishOr`: Direct nullish-terminology rename; null and undefined mapper results are still discarded.

- `Array.fromNullable` -> `Array.fromNullishOr`: Direct nullish-terminology rename; null and undefined become an empty array and other values become a singleton.

- `Array.getEquivalence` -> `Array.makeEquivalence`: Direct rename; pass the element Equivalence unchanged.

- `Array.getLefts` -> `Array.getFailures`: Either became Result; this extracts failure payloads in input order.

- `Array.getOrder` -> `Array.makeOrder`: Direct rename; pass the element Order unchanged.

- `Array.getRights` -> `Array.getSuccesses`: Either became Result; this extracts success payloads in input order.

- `Array.init` -> `Array.init`: The API and Option\<Array\<A\>\> behavior remain unchanged.

- `Array.isEmptyArray` -> `Array.isArrayEmpty`: Direct word-order rename; retains the mutable empty-array type guard.

- `Array.isEmptyReadonlyArray` -> `Array.isReadonlyArrayEmpty`: Direct word-order rename; retains the readonly empty-array type guard.

- `Array.isNonEmptyArray` -> `Array.isArrayNonEmpty`: Direct word-order rename; retains the mutable NonEmptyArray type guard.

- `Array.isNonEmptyReadonlyArray` -> `Array.isReadonlyArrayNonEmpty`: Direct word-order rename; retains the NonEmptyReadonlyArray type guard.

- `Array.liftEither` -> `Array.liftResult`: Either became Result; failures produce an empty array and successes produce a singleton.

- `Array.liftNullable` -> `Array.liftNullishOr`: Direct nullish-terminology rename; the lifted function still returns zero or one element.

- `Array.modifyNonEmptyHead` -> `Array.modifyHeadNonEmpty`: Direct word-order rename with the same non-empty-preserving result.

- `Array.modifyNonEmptyLast` -> `Array.modifyLastNonEmpty`: Direct word-order rename with the same non-empty-preserving result.

- `Array.modifyOption` -> `Array.modify`: The Option suffix was dropped; an out-of-bounds index still returns Option.none.

- `Array.partition` -> `Array.partition`: Return Result.succeed(value) for a match and Result.fail(value) otherwise; Filter.fromPredicate adapts predicates that only inspect the value. The tuple is now [passes, fails], the reverse of v3 [excluded, satisfying].

- `Array.partitionMap` -> `Array.partition`: Pass a Result-returning mapper instead of Either. The tuple is [passes, fails], the reverse of v3 [lefts, rights].

- `Array.removeOption` -> `Array.remove`: The closest API now returns an unchanged copy out of bounds; use Array.get before Array.remove to preserve the old Option result.

- `Array.replaceOption` -> `Array.replace`: The Option suffix was dropped; an out-of-bounds index still returns Option.none.

- `Array.separate` -> `Array.separate`: The tuple is now [successes, failures], the reverse of v3 [lefts, rights].

- `Array.setNonEmptyHead` -> `Array.setHeadNonEmpty`: Direct word-order rename with the same non-empty-preserving result.

- `Array.setNonEmptyLast` -> `Array.setLastNonEmpty`: Direct word-order rename with the same non-empty-preserving result.

- `Array.splitNonEmptyAt` -> `Array.splitAtNonEmpty`: Direct word-order rename; the left output remains guaranteed non-empty.

- `Array.tail` -> `Array.tail`: The API and Option\<Array\<A\>\> behavior remain unchanged.

- `Array.unsafeGet` -> `Array.getUnsafe`: Direct word-order rename; it still throws for an out-of-bounds index.
