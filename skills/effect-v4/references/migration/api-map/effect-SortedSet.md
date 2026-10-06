# v3 → v4 API map: effect/SortedSet

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/SortedSet`

- `SortedSet.SortedSet` -> `HashSet.HashSet`: Use HashSet as the immutable core model and retain Order externally; ordered iteration requires sorting on observation.

- `SortedSet.TypeId` -> `none`: The SortedSet brand was removed and HashSet.TypeId is private; use HashSet.isHashSet when a guard is needed.

- `SortedSet.add` -> `HashSet.add`: Direct persistent add on the replacement set; sort only when traversing.

- `SortedSet.difference` -> `HashSet.difference + HashSet.fromIterable`: Convert the old general iterable argument to HashSet before taking the difference.

- `SortedSet.empty` -> `HashSet.empty`: SortedSet was removed; use an immutable HashSet and retain the element Order separately.

- `SortedSet.every` -> `HashSet.every`: Run the predicate against the replacement HashSet; sort first only if traversal order has observable effects.

- `SortedSet.filter` -> `HashSet.filter`: Direct persistent filtering on the replacement set; traversal is unordered until explicitly sorted.

- `SortedSet.flatMap`: TODO: needs guidance

- `SortedSet.fromIterable` -> `HashSet.fromIterable`: Use HashSet.fromIterable and retain the element Order separately.

- `SortedSet.getEquivalence` -> `Equal.asEquivalence`: HashSet implements Effect equality by set content; use Equal.asEquivalence\<HashSet.HashSet\<A\>\>().

- `SortedSet.has` -> `HashSet.has`: Use direct membership testing on the replacement HashSet.

- `SortedSet.intersection` -> `HashSet.intersection + HashSet.fromIterable`: Convert the old general iterable argument to HashSet before taking the intersection.

- `SortedSet.isSortedSet` -> `HashSet.isHashSet`: Use the replacement model guard; it does not prove that observations were sorted.

- `SortedSet.make` -> `HashSet.make`: Remove the outer order-curried constructor and pass values directly to HashSet.make.

- `SortedSet.map` -> `HashSet.map`: Remove the output Order argument; retain it externally and sort only when traversing.

- `SortedSet.partition` -> `HashSet.filter`: Build [excluded, satisfying] with complementary HashSet.filter calls.

- `SortedSet.remove` -> `HashSet.remove`: Direct persistent removal on the replacement set.

- `SortedSet.size` -> `HashSet.size`: Direct size query on the replacement immutable set.

- `SortedSet.some` -> `HashSet.some`: Run the predicate against the replacement HashSet; sort first only if traversal order has observable effects.

- `SortedSet.toggle` -> `HashSet.has + HashSet.add / HashSet.remove`: HashSet has no toggle; branch on membership and add or remove the element.

- `SortedSet.union` -> `HashSet.union + HashSet.fromIterable`: Convert the old general iterable argument to HashSet before taking the union.

- `SortedSet.values` -> `Array.sort`: Sort the replacement HashSet with the retained Order and iterate the resulting array.
