# v3 → v4 API map: effect/FiberRefsPatch

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/FiberRefsPatch`

- `FiberRefsPatch.Add` -> `Context.add`: FiberRefsPatch was removed; apply Reference overrides directly to Context.

- `FiberRefsPatch.AndThen` -> `Context.merge`: FiberRefsPatch was removed; compose Context updates directly, with later values overriding earlier ones.

- `FiberRefsPatch.Empty` -> `Context.Context<never>`: The empty patch model was removed; an empty Context represents no overrides.

- `FiberRefsPatch.FiberRefsPatch` -> `none`: The patch data type was removed with FiberRefs; construct or merge Context values directly.

- `FiberRefsPatch.combine` -> `Context.merge`: FiberRefsPatch was removed; merge the resulting Context values instead of combining patches.

- `FiberRefsPatch.diff` -> `none`: There is no generic Context diff because FiberRef fork and join patch semantics were removed.

- `FiberRefsPatch.empty` -> `Context.empty`: Use an empty Context when no services or Reference overrides are applied.

- `FiberRefsPatch.patch` -> `Context.merge`: Merge explicit Context overrides into the base Context; FiberId-aware patch application no longer exists.
