# v3 → v4 API map: effect/MetricKey

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MetricKey`

- `MetricKey.MetricKey` -> `Metric.Metric`: Key identity, metadata, and operations are combined in Metric\<Input, State\>.

- `MetricKey.MetricKey.Untyped` -> `Metric.Metric<any, any>`: The separate untyped key alias was removed; use an untyped complete Metric only where required.

- `MetricKey.MetricKey.Variance` -> `Metric.Metric`: There is no separate key variance interface; variance is carried by Metric's Input and State phantom fields.

- `MetricKey.MetricKeyTypeId` -> `none`: The key brand was removed; Metric's protocol key is internal.

- `MetricKey.counter` -> `Metric.counter`: The key and key type were merged into the complete Metric.counter constructor.

- `MetricKey.frequency` -> `Metric.frequency`: The key and key type were merged into the complete Metric.frequency constructor.

- `MetricKey.gauge` -> `Metric.gauge`: The key and key type were merged into the complete Metric.gauge constructor.

- `MetricKey.histogram` -> `Metric.histogram`: The key and key type were merged into the complete Metric.histogram constructor.

- `MetricKey.isMetricKey` -> `Metric.isMetric`: Keys became complete metrics; use the complete-metric runtime guard.

- `MetricKey.summary` -> `Metric.summary`: The key and key type were merged into the complete Metric.summary constructor.

- `MetricKey.taggedWithLabels` -> `Metric.withAttributes`: Labels became attributes. Pass a string record or array of string tuples.
