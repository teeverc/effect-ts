# v3 → v4 API map: effect/RequestResolver

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/RequestResolver`

- `RequestResolver.RequestResolver` -> `RequestResolver.RequestResolver`: The interface remains as RequestResolver\<A extends Request.Any\>; remove its R parameter and move service requirements to Request\<A, E, R\>.

- `RequestResolver.RequestResolver.Variance` -> `RequestResolver.RequestResolver.Variance`: The variance marker remains but tracks only the accepted Request type; resolver environment variance was removed.

- `RequestResolver.RequestResolverTypeId` -> `none`: The resolver type id is internal in v4; use RequestResolver constructors and isRequestResolver rather than depending on its brand.

- `RequestResolver.aroundRequests` -> `RequestResolver.around`: around now receives Request.Entry batches; map entries to entry.request in before and after when hooks need raw request values.

- `RequestResolver.contextFromEffect` -> `Request.Request<A, E, R>`: Resolvers no longer carry an environment parameter; declare R on each Request and use entry.context inside the resolver callback.

- `RequestResolver.contextFromServices` -> `Request.Request<A, E, R>`: Declare the selected services in the Request R parameter and read them from each entry.context; resolver-level context capture was removed.

- `RequestResolver.eitherWith` -> `RequestResolver.fromEffectTagged`: Define one resolver for the combined tagged request union, or use RequestResolver.make to partition entries manually; resolver routing combinators were removed.

- `RequestResolver.locally` -> `Effect.provideService`: FiberRef-based resolver localization was removed; migrate the FiberRef to Context.Reference and provide its value around the request effect or resolver work.

- `RequestResolver.makeBatched` -> `RequestResolver.make`: make now receives a non-empty batch of Request.Entry values; read entry.request and complete every entry with completeUnsafe or Request completion helpers.

- `RequestResolver.makeWithEntry` -> `RequestResolver.make`: Use make for entry-level handling; v4 supplies one non-empty batch and key instead of nested sequential and parallel entry arrays.

- `RequestResolver.mapInputContext` -> `Request.Request<A, E, R>`: Resolver environments were removed; put required services on the Request R parameter and transform or provide each entry.context explicitly when needed.

- `RequestResolver.provideContext` -> `Effect.provideService`: Provide services to Effect.request so they are captured in entry.context; RequestResolver itself no longer has an environment parameter.
