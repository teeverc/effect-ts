# v3 → v4 API map: effect/MetricRegistry

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MetricRegistry`

- `MetricRegistry.MetricRegistry` -> `Metric.MetricRegistry`: The method-bearing registry became a Context.Reference whose service is a Map. Metrics register metadata and hooks lazily.

- `MetricRegistry.MetricRegistryTypeId` -> `none`: The registry service is an ordinary Map behind a Context.Reference and has no public brand.

- `MetricRegistry.make` -> `new Map<string, Metric.Metric.Metadata<any, any>>()`: Provide a fresh Map to Metric.MetricRegistry for isolation. Read it through Metric.snapshot or snapshotUnsafe.
