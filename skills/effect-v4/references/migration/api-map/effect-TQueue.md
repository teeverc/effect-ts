# v3 → v4 API map: effect/TQueue

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TQueue`

- `TQueue.BaseTQueue` -> `TxQueue.TxQueueState`: The shared queue state model was renamed and now includes the richer open, closing, and done lifecycle.

- `TQueue.TDequeue` -> `TxQueue.TxDequeue`: Rename the read-side type; it now carries an error channel.

- `TQueue.TDequeueTypeId` -> `TxQueue.isTxDequeue`: The type id is internal in v4; use the public runtime guard.

- `TQueue.TEnqueue` -> `TxQueue.TxEnqueue`: Rename the write-side type; it now carries an error channel.

- `TQueue.TEnqueueTypeId` -> `TxQueue.isTxEnqueue`: The type id is internal in v4; use the public runtime guard.

- `TQueue.TQueue` -> `TxQueue.TxQueue`: Rename the type; it now carries an error channel and completion lifecycle. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.TQueue.TDequeueVariance` -> `TxQueue.TxDequeue.Variance`: The read-side variance marker moved under TxDequeue and now includes the error type.

- `TQueue.TQueue.TEnqueueVariance` -> `TxQueue.TxEnqueue.Variance`: The write-side variance marker moved under TxEnqueue and now includes the error type.

- `TQueue.bounded` -> `TxQueue.bounded`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.capacity` -> `queue.capacity`: Capacity is now a property on TxQueue handles rather than a module function.

- `TQueue.dropping` -> `TxQueue.dropping`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.isEmpty` -> `TxQueue.isEmpty`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.isFull` -> `TxQueue.isFull`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.isShutdown` -> `TxQueue.isShutdown`: Import TxQueue from "effect/TxQueue"; it checks the richer done lifecycle and returns an ordinary Effect.

- `TQueue.isTDequeue` -> `TxQueue.isTxDequeue`: The runtime guard was renamed with the TxDequeue type.

- `TQueue.isTEnqueue` -> `TxQueue.isTxEnqueue`: The runtime guard was renamed with the TxEnqueue type.

- `TQueue.isTQueue` -> `TxQueue.isTxQueue`: The runtime guard was renamed with the TxQueue type.

- `TQueue.offerAll` -> `TxQueue.offerAll`: The operation remains, but now returns rejected elements rather than a boolean.

- `TQueue.peek` -> `TxQueue.peek`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.poll` -> `TxQueue.poll`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.seek` -> `none`: TxQueue has no seek helper; repeat TxQueue.take under Effect.tx until the predicate matches.

- `TQueue.shutdown` -> `TxQueue.shutdown`: The operation remains, but now returns whether shutdown changed the queue state.

- `TQueue.size` -> `TxQueue.size`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.sliding` -> `TxQueue.sliding`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.take` -> `TxQueue.take`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.takeAll` -> `TxQueue.takeAll`: The operation now blocks until at least one item is available, returns a NonEmptyArray, and propagates the queue error channel through an ordinary Effect.

- `TQueue.takeBetween` -> `TxQueue.takeBetween`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.takeN` -> `TxQueue.takeN`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TQueue.unbounded` -> `TxQueue.unbounded`: Import TxQueue from "effect/TxQueue"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.
