# v3 → v4 API map: effect/Request

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Request`

- `Request.Cache` -> `RequestResolver.asCache`: The runtime request cache type was removed; expose resolver results through a first-class Cache, or use RequestResolver.withCache to retain a resolver.

- `Request.Entry` -> `Request.Entry`: Entry remains but now carries request, context, uninterruptible, and completeUnsafe fields; Deferred, listener, owner, and state fields were removed.

- `Request.EntryTypeId` -> `none`: Request entries are unbranded structural values in v4; do not inspect or construct an entry type id.

- `Request.Listeners` -> `none`: Request listener accounting is no longer public; cancellation and shared request lifecycle are managed by the v4 runtime and resolver caching.

- `Request.Request` -> `Request.Request`: The request model remains and adds a third R parameter for services required while resolving the request.

- `Request.Request.OptionalResult` -> `Exit.Exit<Option.Option<Request.Success<R>>, Request.Error<R>>`: The named alias was removed; write the optional request exit type directly when it is still required.

- `Request.RequestTypeId` -> `none`: The request type id is internal in v4; define requests by extending Request.Request or with Request.Class and do not depend on branding internals.

- `Request.interruptWhenPossible` -> `none`: Request cancellation is managed by the v4 batching runtime; resolver code should complete the entries it receives and not wrap work with this internal listener helper.

- `Request.isEntry` -> `none`: The entry guard was removed; entries are supplied structurally to RequestResolver callbacks.

- `Request.makeCache` -> `RequestResolver.asCache`: Create a cache from a resolver with capacity and timeToLive options, or use RequestResolver.withCache for a cached resolver.
