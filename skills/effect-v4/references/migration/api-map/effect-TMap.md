# v3 → v4 API map: effect/TMap

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TMap`

- `TMap.TMap` -> `TxHashMap.TxHashMap`: Rename the type and import from "effect/TxHashMap". V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.TMap.Variance` -> `none`: TxHashMap exposes no public variance marker.

- `TMap.TMapTypeId` -> `TxHashMap.isTxHashMap`: The type id is internal in v4; use the public runtime guard.

- `TMap.empty` -> `TxHashMap.empty`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.findAllSTM` -> `TxHashMap.entries + Effect traversal`: No effectful mapped-find helper remains; traverse the entry snapshot explicitly inside Effect.tx.

- `TMap.findSTM` -> `TxHashMap.entries + Effect.findFirst`: No effectful mapped-find helper remains; traverse entries explicitly inside Effect.tx.

- `TMap.fromIterable` -> `TxHashMap.fromIterable`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.get` -> `TxHashMap.get`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.getOrElse` -> `Effect.map(TxHashMap.get(self, key), Option.getOrElse(fallback))`: Compose the retained optional get operation with Option.getOrElse.

- `TMap.has` -> `TxHashMap.has`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.isEmpty` -> `TxHashMap.isEmpty`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.keys` -> `TxHashMap.keys`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.make` -> `TxHashMap.make`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.reduce` -> `TxHashMap.reduce`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.reduceSTM` -> `TxHashMap.entries + Effect.reduce`: Snapshot entries and reduce them effectfully inside the surrounding Effect.tx transaction.

- `TMap.remove` -> `TxHashMap.remove`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.removeAll` -> `TxHashMap.removeMany`: The bulk removal operation was renamed.

- `TMap.set`: TODO: needs guidance

- `TMap.setIfAbsent` -> `Effect.tx + TxHashMap.get/TxHashMap.set`: No direct helper remains; check and conditionally set under one outer transaction.

- `TMap.size` -> `TxHashMap.size`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TMap.takeFirst` -> `none`: No atomic take-and-match helper exists in TxHashMap; implement explicit selection and removal inside Effect.tx.

- `TMap.takeFirstSTM` -> `none`: No effectful atomic take-and-match helper exists; implement explicit traversal and removal inside Effect.tx.

- `TMap.takeSome` -> `none`: No atomic multi-take helper exists in TxHashMap; implement explicit selection and removals inside Effect.tx.

- `TMap.takeSomeSTM` -> `none`: No effectful atomic multi-take helper exists; implement explicit traversal and removals inside Effect.tx.

- `TMap.toArray` -> `TxHashMap.entries`: Use the entry snapshot; it replaces the old array conversion.

- `TMap.toChunk` -> `Effect.map(TxHashMap.entries(self), Chunk.fromIterable)`: Convert the entry snapshot to Chunk explicitly.

- `TMap.toHashMap` -> `TxHashMap.snapshot`: The immutable HashMap snapshot operation was renamed.

- `TMap.toMap` -> `Effect.map(TxHashMap.entries(self), (entries) => new Map(entries))`: Build a JavaScript Map from the entry snapshot.

- `TMap.transform` -> `TxHashMap.map`: V4 map returns a new map rather than mutating self; key-changing transforms require snapshot and rebuild logic.

- `TMap.transformSTM` -> `TxHashMap.entries + Effect traversal + TxHashMap.fromIterable`: No in-place effectful transform remains; traverse a snapshot and rebuild inside Effect.tx.

- `TMap.transformValues` -> `TxHashMap.map`: V4 map transforms values but returns a new map rather than mutating self.

- `TMap.transformValuesSTM` -> `TxHashMap.entries + Effect traversal + TxHashMap.fromIterable`: No effectful map remains; traverse a snapshot and rebuild inside Effect.tx.

- `TMap.updateWith` -> `TxHashMap.modifyAt`: modifyAt is the closest atomic keyed update, but returns void; preserve any old return value explicitly if needed.

- `TMap.values` -> `TxHashMap.values`: Import TxHashMap from "effect/TxHashMap"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.
