# v3 → v4 API map: effect/DefaultServices

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/DefaultServices`

- `DefaultServices.DefaultServices` -> `none`: The aggregate type and module were removed; Clock, Console, Random, ConfigProvider, and Tracer are independent defaulted references.

- `DefaultServices.currentServices` -> `Effect.context() and Context.get(context, reference)`: The aggregate FiberRef was removed; access and override default Context.Reference services individually.

- `DefaultServices.liveServices` -> `Context.empty() with individual Context.Reference defaults`: There is no live-services bundle; each default service reference supplies its own live default.
