# v3 → v4 API map: effect/GroupBy

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/GroupBy`

- `GroupBy.GroupBy` -> `Stream<readonly [K, Stream<V>]>`: The GroupBy datatype is removed in v4; Stream.groupBy/groupByKey now return an ordinary Stream of readonly [key, substream] pairs, processed with regular Stream operators.

- `GroupBy.GroupBy.Variance` -> `none`: Variance plumbing for the removed GroupBy datatype; v4 has no GroupBy type, so there is no variance interface to migrate to.

- `GroupBy.GroupByTypeId` -> `none`: Brand symbol for the removed GroupBy datatype; v4 groupBy results are plain Streams, discriminated with Stream.isStream if needed.

#### `GroupBy.evaluate`

**Replacement:** `Stream.flatMap`

Apply the per-group function over the [key, stream] pairs with Stream.flatMap (or Stream.mapEffect for an effectful result per group), using { concurrency: "unbounded" } to reproduce v3's parallel-groups/arbitrary-merge-order behavior; the v3 bufferSize option moved onto Stream.groupBy itself.

**Example**

```ts
// v3: stream.pipe(Stream.groupByKey(f), GroupBy.evaluate((key, s) => g(key, s)))
stream.pipe(
  Stream.groupByKey(f),
  Stream.flatMap(([key, s]) => g(key, s), { concurrency: "unbounded" })
)

```

- `GroupBy.filter` -> `Stream.filter`: Filter the groups by key with an ordinary Stream.filter on the pairs: Stream.filter(([key]) =\> predicate(key)).

- `GroupBy.first` -> `Stream.take`: Keep only the first n groups with an ordinary Stream.take(n) on the [key, stream] pair stream.

- `GroupBy.make` -> `none`: No wrapper to construct in v4: a grouped stream is just any Stream\<readonly [K, Stream\<V\>]\>, so build the pair stream directly (Stream.groupBy/groupByKey produce it); the v3 shape Stream\<[K, Dequeue\<Take\<V, E\>\>]\> is gone along with the queue-of-Take encoding.
