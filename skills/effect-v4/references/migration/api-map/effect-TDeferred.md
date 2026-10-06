# v3 → v4 API map: effect/TDeferred

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TDeferred`

- `TDeferred.TDeferred` -> `TxDeferred.TxDeferred`: Rename the type and import from "effect/TxDeferred". V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TDeferred.TDeferred.Variance` -> `none`: TxDeferred exposes no public variance marker.

- `TDeferred.TDeferredTypeId` -> `TxDeferred.isTxDeferred`: The type id is internal in v4; use the public runtime guard.

- `TDeferred.await` -> `TxDeferred.await`: Import TxDeferred from "effect/TxDeferred"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TDeferred.make` -> `TxDeferred.make`: Import TxDeferred from "effect/TxDeferred"; the operation keeps its name. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.
