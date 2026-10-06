# v3 → v4 API map: effect/List

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/List`

- `List.Cons` -> `Array.NonEmptyReadonlyArray`: Use the immutable non-empty array type; constructors may return the assignable mutable NonEmptyArray subtype.

- `List.List` -> `ReadonlyArray`: Replace the persistent linked-list representation with ReadonlyArray\<A\>.

- `List.List.AndNonEmpty` -> `Array.ReadonlyArray.AndNonEmpty`: Use the corresponding readonly-array utility type.

- `List.List.OrNonEmpty` -> `Array.ReadonlyArray.OrNonEmpty`: Use the corresponding readonly-array utility type.

- `List.List.With` -> `Array.ReadonlyArray.With`: Use the corresponding readonly-array utility type.

- `List.Nil` -> `none`: Represent this case as readonly []; there is no tagged Nil interface in v4.

- `List.TypeId` -> `none`: Arrays have no List runtime marker; remove TypeId inspection.

- `List.append` -> `Array.append`: List was removed; use Array.append. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.appendAll` -> `Array.appendAll`: List was removed; use Array.appendAll. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.cons` -> `Array.prepend`: List was removed; change List.cons(head, tail) to Array.prepend(tail, head).

- `List.empty` -> `Array.empty`: List was removed; use Array.empty. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.every` -> `Array.every`: List was removed; run the predicate against the replacement array with Array.every.

- `List.filter` -> `Array.filter`: List was removed; use Array.filter. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.filterMap` -> `Array.filterMap`: List was removed; use Array.filterMap and change the callback from Option to Result.

- `List.flatMap`: TODO: needs guidance

- `List.fromIterable` -> `Array.fromIterable`: List was removed; use Array.fromIterable. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.getEquivalence` -> `Array.makeEquivalence`: List was removed; compare the replacement arrays with Array.makeEquivalence.

- `List.head` -> `Array.head`: List was removed; use Array.head. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.isCons` -> `Array.isReadonlyArrayNonEmpty`: List was removed; this checks that the replacement readonly array is non-empty.

- `List.isList` -> `Array.isArray`: The List brand is gone; this now recognizes the replacement JavaScript array representation.

- `List.isNil` -> `Array.isReadonlyArrayEmpty`: List was removed; this checks that the replacement readonly array is empty.

- `List.last` -> `Array.last`: List was removed; use Array.last. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.make` -> `Array.make`: List was removed; use Array.make. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.map` -> `Array.map`: List was removed; use Array.map. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.nil` -> `Array.empty`: List was removed; represent Nil with an empty array.

- `List.of` -> `Array.of`: List was removed; use Array.of. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.partition` -> `Array.partition`: Use a Result-returning callback: success values form the first array and failure values the second.

- `List.partitionMap` -> `Array.partition`: Migrate the Either-returning mapper to Result; successes form the first array and failures the second.

- `List.prependAll` -> `Array.prependAll`: List was removed; use Array.prependAll. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.prependAllReversed` -> `Array.prependAll + Array.reverse`: Use Array.prependAll(self, Array.reverse(prefix)) to preserve the old ordering.

- `List.reduce` -> `Array.reduce`: List was removed; use Array.reduce. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.reduceRight` -> `Array.reduceRight`: List was removed; use Array.reduceRight. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.reverse` -> `Array.reverse`: List was removed; use Array.reverse. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.size` -> `Array.length`: List was removed; use the replacement array length helper or the .length property.

- `List.some` -> `Array.some`: List was removed; run the predicate against the replacement array with Array.some.

- `List.splitAt` -> `Array.splitAt`: List was removed; use Array.splitAt. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.tail` -> `Array.tail`: List was removed; use Array.tail. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.take` -> `Array.take`: List was removed; use Array.take. It preserves ordering but returns arrays rather than persistent linked lists.

- `List.toArray` -> `Array.fromIterable`: After migrating the representation this is usually unnecessary; use Array.fromIterable when a fresh mutable array is required.

- `List.toChunk` -> `Chunk.fromIterable`: Convert the replacement array or other iterable with Chunk.fromIterable.

- `List.unsafeHead` -> `Array.headNonEmpty`: Use a NonEmptyReadonlyArray proof before accessing the head; the v4 helper does not accept an empty array.

- `List.unsafeLast` -> `Array.lastNonEmpty`: Use a NonEmptyReadonlyArray proof before accessing the last element; the v4 helper does not accept an empty array.

- `List.unsafeTail` -> `Array.tailNonEmpty`: Use a NonEmptyReadonlyArray proof before taking the tail; the v4 helper does not accept an empty array.
