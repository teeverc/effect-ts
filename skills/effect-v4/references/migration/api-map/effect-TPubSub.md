# v3 → v4 API map: effect/TPubSub

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TPubSub`

- `TPubSub.TPubSub` -> `TxPubSub.TxPubSub`: Rename the type and import from "effect/TxPubSub". V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TPubSub.TPubSubTypeId` -> `TxPubSub.isTxPubSub`: The type id is internal in v4; use the public runtime guard.

- `TPubSub.bounded` -> `TxPubSub.bounded`: Import TxPubSub from "effect/TxPubSub"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TPubSub.capacity` -> `TxPubSub.capacity`: Import TxPubSub from "effect/TxPubSub"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TPubSub.dropping` -> `TxPubSub.dropping`: Import TxPubSub from "effect/TxPubSub"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TPubSub.isEmpty` -> `TxPubSub.isEmpty`: Import TxPubSub from "effect/TxPubSub"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TPubSub.isFull` -> `TxPubSub.isFull`: Import TxPubSub from "effect/TxPubSub"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TPubSub.isShutdown` -> `TxPubSub.isShutdown`: Import TxPubSub from "effect/TxPubSub"; the operation now returns an ordinary Effect.

- `TPubSub.shutdown` -> `TxPubSub.shutdown`: Import TxPubSub from "effect/TxPubSub"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TPubSub.size` -> `TxPubSub.size`: Import TxPubSub from "effect/TxPubSub"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TPubSub.sliding` -> `TxPubSub.sliding`: Import TxPubSub from "effect/TxPubSub"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TPubSub.subscribeScoped` -> `TxPubSub.subscribe`: The scoped subscription constructor lost its Scoped suffix; it still requires Scope and returns a TxQueue.

- `TPubSub.unbounded` -> `TxPubSub.unbounded`: Import TxPubSub from "effect/TxPubSub"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.
