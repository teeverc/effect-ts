# v3 → v4 API map: effect/TArray

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TArray`

- `TArray.TArray` -> `TxChunk.TxChunk`: TArray has no direct v4 counterpart; TxChunk is the closest rewrite target but uses whole-Chunk operations.

- `TArray.TArray.Variance` -> `none`: TArray was removed and TxChunk exposes no public variance marker.

- `TArray.TArrayTypeId` -> `TxChunk.isTxChunk`: TArray and its public type id were removed; use the TxChunk runtime guard after rewriting the data structure.

- `TArray.collectFirst` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.collectFirstSTM` -> `Effect.tx + TxChunk.get + Effect traversal`: TArray was removed. Read the TxChunk snapshot and perform the effectful traversal explicitly within the same Effect.tx transaction.

- `TArray.contains` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.count` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.countSTM` -> `Effect.tx + TxChunk.get + Effect traversal`: TArray was removed. Read the TxChunk snapshot and perform the effectful traversal explicitly within the same Effect.tx transaction.

- `TArray.empty` -> `TxChunk.empty`: TArray was removed; TxChunk is the closest v4 transactional indexed collection.

- `TArray.every` -> `Effect.map(TxChunk.get(self), Chunk.every(predicate))`: TArray was removed; read the TxChunk snapshot and test every element inside the surrounding Effect.tx transaction.

- `TArray.everySTM` -> `Effect.tx + TxChunk.get + Effect traversal`: TArray was removed. Read the TxChunk snapshot and perform the effectful traversal explicitly within the same Effect.tx transaction.

- `TArray.findFirstIndex` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.findFirstIndexFrom` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.findFirstIndexWhere` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.findFirstIndexWhereFrom` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.findFirstIndexWhereFromSTM` -> `Effect.tx + TxChunk.get + Effect traversal`: TArray was removed. Read the TxChunk snapshot and perform the effectful traversal explicitly within the same Effect.tx transaction.

- `TArray.findFirstIndexWhereSTM` -> `Effect.tx + TxChunk.get + Effect traversal`: TArray was removed. Read the TxChunk snapshot and perform the effectful traversal explicitly within the same Effect.tx transaction.

- `TArray.findFirstSTM` -> `Effect.tx + TxChunk.get + Effect traversal`: TArray was removed. Read the TxChunk snapshot and perform the effectful traversal explicitly within the same Effect.tx transaction.

- `TArray.findLast` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.findLastIndex` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.findLastIndexFrom` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.findLastSTM` -> `Effect.tx + TxChunk.get + Effect traversal`: TArray was removed. Read the TxChunk snapshot and perform the effectful traversal explicitly within the same Effect.tx transaction.

- `TArray.fromIterable` -> `TxChunk.fromIterable`: TArray was removed; construct the v4 TxChunk rewrite target from the iterable.

- `TArray.get` -> `Effect.map(TxChunk.get(self), Chunk.get(index))`: TxChunk.get returns the whole Chunk, so apply Chunk.get to preserve indexed optional lookup.

- `TArray.headOption` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.lastOption` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.make` -> `TxChunk.fromIterable(elements)`: TxChunk.make takes one Chunk rather than variadic elements; TxChunk.fromIterable preserves the old call shape after collecting arguments.

- `TArray.maxOption` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.minOption` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.reduce` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.reduceOption` -> `TxChunk.get + Chunk/Array operation`: TArray was removed. Read the TxChunk snapshot and perform the equivalent pure collection query inside the surrounding Effect.tx transaction.

- `TArray.reduceOptionSTM` -> `Effect.tx + TxChunk.get + Effect traversal`: TArray was removed. Read the TxChunk snapshot and perform the effectful traversal explicitly within the same Effect.tx transaction.

- `TArray.reduceSTM` -> `Effect.tx + TxChunk.get + Effect traversal`: TArray was removed. Read the TxChunk snapshot and perform the effectful traversal explicitly within the same Effect.tx transaction.

- `TArray.size` -> `TxChunk.size`: TxChunk is the closest v4 rewrite target. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TArray.some` -> `Effect.map(TxChunk.get(self), Chunk.some(predicate))`: TArray was removed; read the TxChunk snapshot and test for a matching element inside the surrounding Effect.tx transaction.

- `TArray.someSTM` -> `Effect.tx + TxChunk.get + Effect traversal`: TArray was removed. Read the TxChunk snapshot and perform the effectful traversal explicitly within the same Effect.tx transaction.

- `TArray.toArray` -> `Effect.map(TxChunk.get(self), Chunk.toArray)`: TxChunk.get returns a Chunk; convert that snapshot to an Array explicitly.

- `TArray.transform` -> `TxChunk.update(self, Chunk.map(f))`: TArray was removed; transform the whole TxChunk snapshot with a Chunk mapping function.

- `TArray.transformSTM` -> `Effect.tx + TxChunk.get/TxChunk.set`: Read the snapshot, traverse it effectfully, and write the rebuilt Chunk within one Effect.tx transaction.

- `TArray.update` -> `TxChunk.modify`: TxChunk updates the whole Chunk; use modify to update the indexed element and preserve the old optional-index behavior.

- `TArray.updateSTM` -> `Effect.tx + TxChunk.get/TxChunk.set`: Read, effectfully update the indexed element, and write the rebuilt Chunk within one Effect.tx transaction.
