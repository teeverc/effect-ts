# v3 → v4 API map: effect/TSet

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TSet`

- `TSet.TSet` -> `TxHashSet.TxHashSet`: Rename the type and import from "effect/TxHashSet". V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSet.TSet.Variance` -> `none`: TxHashSet exposes no public variance marker.

- `TSet.TSetTypeId` -> `TxHashSet.isTxHashSet`: The type id is internal in v4; use the public runtime guard.

- `TSet.difference` -> `TxHashSet.difference`: The name remains, but v4 returns a new set instead of mutating self.

- `TSet.empty` -> `TxHashSet.empty`: Import TxHashSet from "effect/TxHashSet"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSet.fromIterable` -> `TxHashSet.fromIterable`: Import TxHashSet from "effect/TxHashSet"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSet.has` -> `TxHashSet.has`: Import TxHashSet from "effect/TxHashSet"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSet.intersection` -> `TxHashSet.intersection`: The name remains, but v4 returns a new set instead of mutating self.

- `TSet.isEmpty` -> `TxHashSet.isEmpty`: Import TxHashSet from "effect/TxHashSet"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSet.make` -> `TxHashSet.make`: Import TxHashSet from "effect/TxHashSet"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSet.reduce` -> `TxHashSet.reduce`: Import TxHashSet from "effect/TxHashSet"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSet.reduceSTM` -> `TxHashSet.toHashSet + Effect.reduce`: Snapshot the set and reduce effectfully inside the surrounding Effect.tx transaction.

- `TSet.remove` -> `TxHashSet.remove`: The name remains, but v4 returns whether the value existed.

- `TSet.removeAll` -> `Effect.forEach(values, (value) => TxHashSet.remove(self, value))`: No bulk removal helper remains; remove each value inside one outer Effect.tx transaction.

- `TSet.size` -> `TxHashSet.size`: Import TxHashSet from "effect/TxHashSet"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSet.takeFirst` -> `none`: No atomic take-and-match helper exists in TxHashSet; select and remove explicitly inside Effect.tx.

- `TSet.takeFirstSTM` -> `none`: No effectful atomic take-and-match helper exists; traverse and remove explicitly inside Effect.tx.

- `TSet.takeSome` -> `none`: No atomic multi-take helper exists in TxHashSet; select and remove explicitly inside Effect.tx.

- `TSet.takeSomeSTM` -> `none`: No effectful atomic multi-take helper exists; traverse and remove explicitly inside Effect.tx.

- `TSet.toArray` -> `Effect.map(TxHashSet.toHashSet(self), Array.from)`: Convert the immutable HashSet snapshot to an Array explicitly.

- `TSet.toChunk` -> `Effect.map(TxHashSet.toHashSet(self), (set) => Chunk.fromIterable(set))`: Convert the immutable HashSet snapshot to Chunk explicitly.

- `TSet.toReadonlySet` -> `Effect.map(TxHashSet.toHashSet(self), (set) => new Set(set))`: Convert the immutable HashSet snapshot to a JavaScript ReadonlySet explicitly.

- `TSet.transform` -> `TxHashSet.map`: The closest helper returns a new set instead of mutating self.

- `TSet.transformSTM` -> `TxHashSet.toHashSet + Effect traversal + TxHashSet.fromIterable`: No effectful transform remains; traverse a snapshot and rebuild inside Effect.tx.

- `TSet.union` -> `TxHashSet.union`: The name remains, but v4 returns a new set instead of mutating self.
