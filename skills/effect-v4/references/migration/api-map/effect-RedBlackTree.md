# v3 → v4 API map: effect/RedBlackTree

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/RedBlackTree`

- `RedBlackTree.Direction` -> `none`: The tree direction type was removed; use normal array order or Array.reverse.

- `RedBlackTree.RedBlackTree` -> `ReadonlyArray<readonly [K, V]>`: The core tree was removed; use sorted immutable entries for small collections or an external persistent ordered multimap when complexity or duplicate-key semantics matter.

- `RedBlackTree.RedBlackTree.Direction` -> `none`: The nested direction type was removed; use normal array order or Array.reverse.

- `RedBlackTree.TypeId` -> `none`: The RedBlackTree module and brand symbol were removed.

- `RedBlackTree.at` -> `Array.drop`: Represent the removed tree as sorted entries; for a non-negative index, Array.drop(entries, index) traverses forward from that absolute position.

- `RedBlackTree.atReversed` -> `Array.take + Array.reverse`: For a valid absolute index, reverse Array.take(entries, index + 1) to traverse backward from it.

- `RedBlackTree.empty` -> `Array.empty`: The module was removed; use an empty Array\<readonly [K, V]\> and retain the Order separately.

- `RedBlackTree.first` -> `Array.head`: On a sorted entry array, Array.head returns the same optional minimum entry.

- `RedBlackTree.forEachBetween` -> `Array.filter + Array.forEach`: Filter sorted entries to min \<= key \< max with the retained Order, then visit them with Array.forEach.

- `RedBlackTree.forEachGreaterThanEqual` -> `Array.filter + Array.forEach`: Filter sorted entries to key \>= min with the retained Order, then visit them in ascending order.

- `RedBlackTree.forEachLessThan` -> `Array.filter + Array.forEach`: Filter sorted entries to key \< max with the retained Order, then visit them in ascending order.

- `RedBlackTree.fromIterable` -> `Array.sortWith`: Sort the entry iterable by key and retain the Order separately; this does not preserve logarithmic tree operations.

- `RedBlackTree.getAt` -> `Array.get`: Array.get on sorted entries preserves the optional index lookup behavior.

- `RedBlackTree.getOrder` -> `none`: No replacement collection stores an Order; retain and pass the Order explicitly.

- `RedBlackTree.greaterThan` -> `Array.filter`: Filter sorted entries with the retained Order for key \> bound.

- `RedBlackTree.greaterThanEqual` -> `Array.filter`: Filter sorted entries with the retained Order for key \>= bound.

- `RedBlackTree.greaterThanEqualReversed` -> `Array.filter + Array.reverse`: Filter sorted entries with the retained Order for key \>= bound, then reverse for descending traversal.

- `RedBlackTree.greaterThanReversed` -> `Array.filter + Array.reverse`: Filter sorted entries with the retained Order for key \> bound, then reverse for descending traversal.

- `RedBlackTree.has` -> `Array.some`: Use Array.some on sorted entries with Equal.equals for key membership; this is linear rather than logarithmic.

- `RedBlackTree.insert` -> `Array.prepend + Array.sortWith`: Prepend the entry and sort by key to preserve newest-first comparator ties; use an external ordered multimap if logarithmic updates matter.

- `RedBlackTree.isRedBlackTree` -> `Array.isArray`: The brand was removed; Array.isArray only checks the replacement representation and cannot prove its sorted invariant.

- `RedBlackTree.keys` -> `Array.map`: Map sorted entries to keys and iterate the resulting array.

- `RedBlackTree.keysReversed` -> `Array.reverse + Array.map`: Reverse sorted entries, map them to keys, and iterate the resulting array.

- `RedBlackTree.last` -> `Array.last`: On a sorted entry array, Array.last returns the same optional maximum entry.

- `RedBlackTree.lessThan` -> `Array.filter`: Filter sorted entries with the retained Order for key \< bound.

- `RedBlackTree.lessThanEqual` -> `Array.filter`: Filter sorted entries with the retained Order for key \<= bound.

- `RedBlackTree.lessThanEqualReversed` -> `Array.filter + Array.reverse`: Filter sorted entries with the retained Order for key \<= bound, then reverse for descending traversal.

- `RedBlackTree.lessThanReversed` -> `Array.filter + Array.reverse`: Filter sorted entries with the retained Order for key \< bound, then reverse for descending traversal.

- `RedBlackTree.make` -> `Array.sortWith`: Sort the supplied entries by key and retain the Order separately; this is not a balanced tree.

- `RedBlackTree.reduce` -> `Array.reduce`: Reduce sorted entries in ascending order, adapting the callback to receive [key, value].

- `RedBlackTree.removeFirst` -> `Array.findFirstIndex + Array.remove`: Find the first entry whose key is Equal.equals to the target, then remove that index; leave the array unchanged when absent.

- `RedBlackTree.reversed` -> `Array.reverse`: Reverse the sorted entry array for descending traversal.

- `RedBlackTree.size` -> `Array.length`: Use Array.length or the .length property on the replacement entry array.

- `RedBlackTree.values` -> `Array.map`: Map sorted entries to values and iterate the resulting array to preserve key order.

- `RedBlackTree.valuesReversed` -> `Array.reverse + Array.map`: Reverse sorted entries, map them to values, and iterate to preserve reverse key order.
