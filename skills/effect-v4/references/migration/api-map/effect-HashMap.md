# v3 → v4 API map: effect/HashMap

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/HashMap`

- `HashMap.HashMap` -> `HashMap.HashMap`: The immutable two-parameter model remains; use public operations rather than depending on its representation.

- `HashMap.TypeId` -> `HashMap.isHashMap`: The brand is private; use HashMap.isHashMap for runtime refinement and HashMap.HashMap\<K, V\> in type positions.

- `HashMap.countBy` -> `HashMap.reduce`: Count matches with HashMap.reduce(self, 0, (count, value, key) =\> count + (predicate(value, key) ? 1 : 0)).

- `HashMap.keySet` -> `HashSet.fromIterable + HashMap.keys`: Construct the set with HashSet.fromIterable(HashMap.keys(self)); no direct keySet helper remains.

- `HashMap.unsafeGet` -> `HashMap.getUnsafe`: Direct word-order rename; it still throws for a missing key.
