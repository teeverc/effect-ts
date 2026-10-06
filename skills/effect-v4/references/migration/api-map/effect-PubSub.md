# v3 → v4 API map: effect/PubSub

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/PubSub`

- `PubSub.PubSub` -> `PubSub.PubSub`: The model remains but no longer extends Queue.Enqueue; replace Queue operations with explicit PubSub.publish, PubSub.publishAll, and PubSub.subscribe calls.
