# v3 → v4 API map: effect/PartitionedSemaphore

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/PartitionedSemaphore`

- `PartitionedSemaphore.PartitionedSemaphore` -> `PartitionedSemaphore.PartitionedSemaphore`: The model remains and now also exposes capacity, available, take, release, withPermit, and conditional permit operations.

- `PartitionedSemaphore.TypeId` -> `PartitionedSemaphore.PartitionedTypeId`: The public type id was renamed to distinguish it from the regular Semaphore type id.
