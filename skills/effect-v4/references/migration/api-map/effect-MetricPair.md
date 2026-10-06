# v3 → v4 API map: effect/MetricPair

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MetricPair`

- `MetricPair.MetricPair` -> `Metric.Metric.Snapshot`: Registry key/state pairs became discriminated snapshots containing id, type, description, attributes, and state.

- `MetricPair.MetricPair.Untyped` -> `Metric.Metric.Snapshot`: Registry key/state pairs became discriminated snapshots containing id, type, description, attributes, and state.

- `MetricPair.MetricPair.Variance` -> `none`: Snapshots are structural, so the pair variance helper and brand symbol were removed.

- `MetricPair.MetricPairTypeId` -> `none`: Snapshots are structural, so the pair variance helper and brand symbol were removed.

- `MetricPair.make` -> `Metric.snapshot`: There is no pair constructor. Obtain snapshots with Metric.snapshot or Metric.snapshotUnsafe; manually constructed data can satisfy Metric.Metric.SnapshotProto.

- `MetricPair.unsafeMake` -> `Metric.snapshot`: There is no pair constructor. Obtain snapshots with Metric.snapshot or Metric.snapshotUnsafe; manually constructed data can satisfy Metric.Metric.SnapshotProto.
