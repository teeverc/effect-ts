# v3 → v4 API map: effect/Reloadable

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Reloadable`

- `Reloadable.Reloadable` -> `LayerRef.LayerRef<I, E>`: LayerRef is the v4 refreshable layer-context abstraction.

- `Reloadable.Reloadable.Variance` -> `none`: The exported variance artifact was removed and LayerRef has no public counterpart.

- `Reloadable.ReloadableTypeId` -> `none`: Reloadable was removed and LayerRef's marker is private.

- `Reloadable.auto` -> `LayerRef.Service(..., { layer, invalidationSchedule: schedule, preload: true }).layer`: Use LayerRef for scheduled refresh; add idleTimeToLive: Duration.infinity to preserve an always-resident instance.

- `Reloadable.autoFromConfig` -> `Layer.unwrap with Effect.contextWith and LayerRef.make`: Compute the schedule from the current context, then construct a preloaded LayerRef; no config-specific constructor remains.

- `Reloadable.get` -> `ServiceRef.get or Effect.map(ServiceRef.contextEffect, Context.get(Service))`: LayerRef.get provides the current context as a layer; contextEffect gives scoped direct access.

- `Reloadable.manual` -> `LayerRef.Service(..., { layer, preload: true }).layer`: Refresh with the generated service's refresh effect; use infinite idleTimeToLive for v3's resident lifecycle.

- `Reloadable.reload` -> `ServiceRef.refresh`: Refresh invalidates and immediately reacquires; invalidate alone rebuilds on the next borrow.

- `Reloadable.reloadFork` -> `ServiceRef.refresh.pipe(Effect.ignore({ log: true }), Effect.forkDetach({ startImmediately: true }), Effect.asVoid)`: This recreates logged, ignored background refresh; forkDaemon became forkDetach.

- `Reloadable.tag` -> `LayerRef.Service<Self>()(id, options)`: The generated LayerRef service class is itself the Context.Service key.
