# v3 → v4 API map: effect/FiberRefs

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/FiberRefs`

- `FiberRefs.FiberRefs` -> `Context.Context`: Fiber-local services and reference overrides are stored directly in the Fiber context in v4.

- `FiberRefs.FiberRefsSym` -> `none`: FiberRefs and its marker symbol were removed.

- `FiberRefs.delete` -> `Context.omit`: FiberRefs became fiber Context; omit a Reference key when constructing the replacement Context.

- `FiberRefs.empty` -> `Context.empty`: Use an empty Context as the starting collection of services and reference overrides.

- `FiberRefs.fiberRefs` -> `none`: Context does not expose public enumeration of its Reference keys; retain the keys explicitly if enumeration is required.

- `FiberRefs.forkAs` -> `none`: Context is inherited automatically when a v4 child fiber is forked; custom per-reference fork patches were removed.

- `FiberRefs.get` -> `Context.getOption`: Read the service as an Option. Context.Reference defaults also produce Some; use Context.getOrUndefined when only stored overrides should count.

- `FiberRefs.getOrDefault` -> `Context.get`: Reads an override or the Context.Reference default value.

- `FiberRefs.joinAs` -> `none`: Child-to-parent FiberRef joining was removed; pass results explicitly or merge ordinary Context values where appropriate.

- `FiberRefs.setAll` -> `Effect.provideContext`: Provide the replacement Context around the Effect that should observe its services and reference overrides.

- `FiberRefs.unsafeMake` -> `Context.empty().pipe(Context.add(...))`: Build a Context from explicit Reference keys and values; FiberId histories and unsafe local maps no longer exist.

- `FiberRefs.updateAs` -> `Context.add`: Add or replace a Reference value in Context; the FiberId parameter and history are removed.

- `FiberRefs.updateManyAs` -> `Context.add`: Apply explicit Context.add calls for each Reference value; FiberId histories and forkAs are removed.
