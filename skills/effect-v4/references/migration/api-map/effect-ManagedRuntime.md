# v3 → v4 API map: effect/ManagedRuntime

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/ManagedRuntime`

- `ManagedRuntime.ManagedRuntime` -> `ManagedRuntime.ManagedRuntime`: The handle remains but is no longer an Effect; runtimeEffect/runtime became contextEffect/context, and make accepts { memoMap }.

- `ManagedRuntime.ManagedRuntime.Context` -> `ManagedRuntime.ManagedRuntime.Services<T>`: The context extractor was renamed to Services.

- `ManagedRuntime.ManagedRuntimeUnify` -> `none`: ManagedRuntime no longer extends Effect, so its unification artifact was removed; call run methods or contextEffect explicitly.

- `ManagedRuntime.ManagedRuntimeUnifyIgnore` -> `none`: ManagedRuntime no longer extends Effect, so the Unify-ignore artifact was removed.

- `ManagedRuntime.TypeId` -> `ManagedRuntime.isManagedRuntime`: The marker is private; use the public guard for runtime narrowing.
