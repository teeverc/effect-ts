# v3 → v4 API map: effect/MutableQueue

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MutableQueue`

- `MutableQueue.EmptyMutableQueue` -> `none`: Queue.poll reports emptiness with Option.none, so no default sentinel is required.

- `MutableQueue.MutableQueue` -> `Queue.Queue`: The replacement Queue is effectful, lifecycle-aware, and not Iterable.

- `MutableQueue.MutableQueue.Empty` -> `none`: Use the Option returned by Queue.poll; the old empty sentinel was removed.

- `MutableQueue.TypeId` -> `none`: The MutableQueue module and its public marker were removed.

- `MutableQueue.bounded` -> `Queue.dropping`: The replacement constructor is effectful; dropping preserves the old immediate rejection when a bounded queue is full.

- `MutableQueue.capacity` -> `none`: Read queue.capacity on the replacement Queue; unbounded queues expose Infinity.

- `MutableQueue.isEmpty` -> `Queue.sizeUnsafe`: Use Queue.sizeUnsafe(queue) === 0, or map the effectful Queue.size result.

- `MutableQueue.isFull` -> `Queue.isFullUnsafe`: Use Queue.isFullUnsafe for synchronous inspection or Queue.isFull for an Effect result.

- `MutableQueue.length` -> `Queue.sizeUnsafe`: Use Queue.sizeUnsafe for synchronous inspection or Queue.size for an Effect result.

- `MutableQueue.offer` -> `Queue.offerUnsafe`: Use with Queue.dropping to preserve the old synchronous boolean rejection at capacity; Queue.offer is the effectful form.

- `MutableQueue.offerAll` -> `Queue.offerAllUnsafe`: The synchronous replacement returns the rejected remainder as an Array; Queue.offerAll is the effectful form.

- `MutableQueue.poll` -> `Queue.poll`: Polling is now effectful and returns Option\<A\> rather than accepting a default; Queue.takeUnsafe is the low-level synchronous alternative.

- `MutableQueue.pollUpTo` -> `Queue.takeUnsafe`: No direct non-blocking take-up-to helper remains; repeatedly call Queue.takeUnsafe and collect successful exits without waiting.

- `MutableQueue.unbounded` -> `Queue.unbounded`: The unbounded replacement constructor is effectful.
