# v3 → v4 API map: effect/RequestBlock

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/RequestBlock`

- `RequestBlock.Empty` -> `none`: The public blocked-request graph was removed; application code should compose Effect.request computations instead of inspecting Empty nodes.

- `RequestBlock.Par` -> `none`: The public blocked-request graph was removed; express parallel request execution with Effect concurrency combinators.

- `RequestBlock.RequestBlock` -> `none`: RequestBlock is no longer public in v4; compose Effect.request values directly and let the runtime batch requests by resolver.

- `RequestBlock.Seq` -> `none`: The public blocked-request graph was removed; express sequencing in the Effect program instead of constructing Seq nodes.

- `RequestBlock.empty` -> `Effect.void`: RequestBlock was removed; represent an empty computation as Effect.void and let Effect.request perform batching.

- `RequestBlock.mapRequestResolvers` -> `Effect.request`: Pass the selected resolver to each Effect.request call; the runtime request graph can no longer be traversed to rewrite resolvers.

- `RequestBlock.parallel` -> `Effect.all`: Compose request effects with Effect.all and explicit concurrency; v4 batching is performed by resolver and batch key rather than RequestBlock nodes.

- `RequestBlock.reduce` -> `none`: The public blocked-request graph and reducer were removed; structure analysis is now internal to the request runtime.

- `RequestBlock.sequential` -> `Effect.andThen`: Sequence request effects with Effect.andThen, flatMap, or generator syntax; RequestBlock sequencing nodes were removed.

- `RequestBlock.single` -> `Effect.request`: Construct the request effect directly with its Request value and RequestResolver; the runtime creates pending entries internally.
