# v3 → v4 API map: effect/MergeStrategy

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MergeStrategy`

- `MergeStrategy.BackPressure` -> `Channel.mergeAll`: Remove the MergeStrategy value and configure the v4 merge directly: backpressure is the default; sliding replacement uses Channel.mergeAll with switch: true or Channel.switchMap.

- `MergeStrategy.BufferSliding` -> `Channel.mergeAll`: Remove the MergeStrategy value and configure the v4 merge directly: backpressure is the default; sliding replacement uses Channel.mergeAll with switch: true or Channel.switchMap.

- `MergeStrategy.MergeStrategy` -> `Channel.mergeAll`: Remove the MergeStrategy value and configure the v4 merge directly: backpressure is the default; sliding replacement uses Channel.mergeAll with switch: true or Channel.switchMap.

- `MergeStrategy.MergeStrategy.Proto` -> `Channel.mergeAll`: Remove the MergeStrategy value and configure the v4 merge directly: backpressure is the default; sliding replacement uses Channel.mergeAll with switch: true or Channel.switchMap.

- `MergeStrategy.MergeStrategyTypeId` -> `Channel.mergeAll`: Remove the MergeStrategy value and configure the v4 merge directly: backpressure is the default; sliding replacement uses Channel.mergeAll with switch: true or Channel.switchMap.

- `MergeStrategy.isBackPressure` -> `Channel.mergeAll`: Remove the MergeStrategy value and configure the v4 merge directly: backpressure is the default; sliding replacement uses Channel.mergeAll with switch: true or Channel.switchMap.

- `MergeStrategy.isBufferSliding` -> `Channel.mergeAll`: Remove the MergeStrategy value and configure the v4 merge directly: backpressure is the default; sliding replacement uses Channel.mergeAll with switch: true or Channel.switchMap.

- `MergeStrategy.isMergeStrategy` -> `Channel.mergeAll`: Remove the MergeStrategy value and configure the v4 merge directly: backpressure is the default; sliding replacement uses Channel.mergeAll with switch: true or Channel.switchMap.

- `MergeStrategy.match` -> `Channel.mergeAll`: Remove the MergeStrategy value and configure the v4 merge directly: backpressure is the default; sliding replacement uses Channel.mergeAll with switch: true or Channel.switchMap.
