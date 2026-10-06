# v3 → v4 API map: effect/Supervisor

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Supervisor`

- `Supervisor.AbstractSupervisor` -> `none`: The ambient Supervisor abstraction and runtime event hooks were removed.

- `Supervisor.Supervisor` -> `none`: Ambient fiber supervision was removed; use structured concurrency or explicit FiberSet and FiberMap tracking.

- `Supervisor.Supervisor.Variance` -> `none`: The Supervisor abstraction and its variance marker were removed.

- `Supervisor.SupervisorTypeId` -> `none`: The Supervisor abstraction and its type identifier were removed.

- `Supervisor.addSupervisor` -> `none`: Layer-installed ambient supervision was removed; use structured concurrency and explicit FiberSet or FiberMap tracking.

- `Supervisor.fibersIn` -> `FiberSet`: Use a scoped FiberSet and explicitly run or add fibers; it does not ambiently observe every descendant.

- `Supervisor.fromEffect` -> `none`: The Supervisor abstraction and its effect-valued observation hook were removed.

- `Supervisor.none` -> `none`: The Supervisor abstraction was removed; normal structured concurrency needs no no-op supervisor.

- `Supervisor.unsafeTrack` -> `FiberSet`: Use scoped FiberSet.make and explicitly run or add fibers; there is no unsafe unscoped ambient tracker.
