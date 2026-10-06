# v3 → v4 API map: effect/SortedMap

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/SortedMap`

- `SortedMap.SortedMap` -> `HashMap.HashMap`: Use HashMap as the immutable core model and retain Order externally; ordered iteration and range seeks require sorting on observation.

- `SortedMap.TypeId` -> `none`: The SortedMap brand was removed and HashMap.TypeId is private; use HashMap.isHashMap when a guard is needed.

- `SortedMap.empty` -> `HashMap.empty`: SortedMap was removed; use an immutable HashMap and retain the key Order separately.

- `SortedMap.entries` -> `HashMap.entries + Array.sortWith`: Materialize HashMap.entries and sort by key with the retained Order when ordered traversal is required.

- `SortedMap.fromIterable` -> `HashMap.fromIterable`: Use HashMap.fromIterable and retain the key Order separately; duplicate keys collapse.

- `SortedMap.get` -> `HashMap.get`: Direct optional lookup on the replacement immutable map.

- `SortedMap.getOrder` -> `none`: HashMap does not store an Order; retain and pass the key Order explicitly.

- `SortedMap.has` -> `HashMap.has`: Use direct membership testing on the replacement HashMap; retain the key Order separately for sorted observations.

- `SortedMap.headOption` -> `HashMap.entries + Array.sortWith + Array.head`: Sort entries by key with the retained Order, then take the optional first entry.

- `SortedMap.isEmpty` -> `HashMap.isEmpty`: Direct emptiness check on the replacement immutable map.

- `SortedMap.isNonEmpty` -> `HashMap.isEmpty`: Use !HashMap.isEmpty(self); no dedicated HashMap.isNonEmpty helper exists.

- `SortedMap.isSortedMap` -> `HashMap.isHashMap`: Use the replacement model guard; it does not prove that observations were sorted.

- `SortedMap.keys` -> `HashMap.entries + Array.sortWith + Array.map`: Sort entries by key, map to keys, and iterate the resulting array.

- `SortedMap.lastOption` -> `HashMap.entries + Array.sortWith + Array.last`: Sort entries by key with the retained Order, then take the optional last entry.

- `SortedMap.make` -> `HashMap.make`: Remove the outer order-curried constructor and pass entries directly to HashMap.make.

- `SortedMap.map` -> `HashMap.map`: The value-and-key callback remains, but result iteration is unordered until explicitly sorted.

- `SortedMap.partition` -> `HashMap.filter`: Build [excluded, satisfying] with complementary HashMap.filter calls; adapt the callback to the old key predicate.

- `SortedMap.reduce` -> `HashMap.reduce`: Reduce the replacement HashMap, but explicitly sort entries first if the old key-order traversal affected the result.

- `SortedMap.remove` -> `HashMap.remove`: Direct persistent removal; explicitly sort only when observing entries.

- `SortedMap.set` -> `HashMap.set`: Direct persistent insert or update; explicitly sort only when observing entries.

- `SortedMap.size` -> `HashMap.size`: Direct size query on the replacement immutable map.

- `SortedMap.values` -> `HashMap.entries + Array.sortWith + Array.map`: Sort entries by key, map to values, and iterate the resulting array.
