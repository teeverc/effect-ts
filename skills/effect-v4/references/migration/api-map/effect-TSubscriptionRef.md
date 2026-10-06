# v3 → v4 API map: effect/TSubscriptionRef

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TSubscriptionRef`

- `TSubscriptionRef.TSubscriptionRef` -> `TxSubscriptionRef.TxSubscriptionRef`: Rename the type; it no longer extends TxRef. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSubscriptionRef.TSubscriptionRef.Variance` -> `none`: TxSubscriptionRef exposes no public variance marker.

- `TSubscriptionRef.TSubscriptionRefTypeId` -> `TxSubscriptionRef.isTxSubscriptionRef`: The type id is internal in v4; use the public runtime guard.

- `TSubscriptionRef.changes` -> `none`: The old unscoped transactional subscription was removed; use scoped TxSubscriptionRef.changes.

- `TSubscriptionRef.changesScoped` -> `TxSubscriptionRef.changes`: The scoped changes operation lost its Scoped suffix and returns a scoped TxQueue.

- `TSubscriptionRef.get` -> `TxSubscriptionRef.get`: Import TxSubscriptionRef from "effect/TxSubscriptionRef"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSubscriptionRef.getAndUpdateSome` -> `TxSubscriptionRef.modify`: Use one atomic modify so successful updates are still published; retain the old value for None.

- `TSubscriptionRef.make` -> `TxSubscriptionRef.make`: Import TxSubscriptionRef from "effect/TxSubscriptionRef"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TSubscriptionRef.modifySome` -> `TxSubscriptionRef.modify`: Use one atomic modify so successful updates are still published; use the fallback result for None.

- `TSubscriptionRef.setAndGet` -> `TxSubscriptionRef.modify`: Use one atomic modify that publishes and returns the newly stored value.

- `TSubscriptionRef.updateSome` -> `TxSubscriptionRef.modify`: Use one atomic modify so updates are published, retaining the old value for None.

- `TSubscriptionRef.updateSomeAndGet` -> `TxSubscriptionRef.modify`: Use one atomic modify that publishes and returns the resulting value, retaining the old value for None.
