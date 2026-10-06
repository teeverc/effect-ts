# v3 → v4 API map: effect/TRef

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TRef`

- `TRef.TRef` -> `TxRef.TxRef`: Rename the type and import from "effect/TxRef". V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TRef.TRef.Variance` -> `none`: TxRef exposes no public variance marker.

- `TRef.TRefTypeId` -> `TxRef.isTxRef`: The type id is internal in v4; use the public runtime guard.

- `TRef.get` -> `TxRef.get`: Import TxRef from "effect/TxRef"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TRef.getAndUpdateSome` -> `TxRef.modify`: Use one atomic modify and keep the old value when the partial update returns None.

- `TRef.make` -> `TxRef.make`: Import TxRef from "effect/TxRef"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TRef.modifySome` -> `TxRef.modify`: Use one atomic modify and return the fallback result when the partial function returns None.

- `TRef.setAndGet` -> `TxRef.modify`: Use one atomic modify that returns and stores the new value.

- `TRef.updateSome` -> `TxRef.modify`: Use one atomic modify and retain the old value when the partial update returns None.

- `TRef.updateSomeAndGet` -> `TxRef.modify`: Use one atomic modify that returns the resulting value, retaining the old value for None.
