# v3 → v4 API map: effect/Subscribable

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Subscribable`

- `Subscribable.Subscribable` -> `custom { readonly get: Effect.Effect<A, E, R>; readonly changes: Stream.Stream<A, E, R> }`: No renamed generic model exists; prefer concrete SubscriptionRef APIs or own this unbranded structural type locally.

- `Subscribable.TypeId` -> `none`: The Subscribable brand has no public replacement; use a concrete model guard or an application structural guard.

- `Subscribable.isSubscribable` -> `none`: The common brand was removed; use a concrete guard such as SubscriptionRef.isSubscriptionRef or an application structural guard.

- `Subscribable.make` -> `object literal { get, changes }`: No generic constructor remains; retain a local structural pair only when both the current read and change stream are needed.

- `Subscribable.map` -> `Effect.map + Stream.map`: For a retained get and changes pair, map the Effect and Stream separately.

- `Subscribable.mapEffect` -> `Effect.flatMap + Stream.mapEffect`: For a retained get and changes pair, flatMap the Effect and mapEffect the Stream separately.

- `Subscribable.unwrap` -> `Effect.flatMap + Stream.unwrap`: Build get with Effect.flatMap and changes with Stream.unwrap; no single v4 helper remains.
