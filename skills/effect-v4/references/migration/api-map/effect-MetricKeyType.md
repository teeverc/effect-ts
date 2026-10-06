# v3 → v4 API map: effect/MetricKeyType

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MetricKeyType`

- `MetricKeyType.CounterKeyTypeTypeId` -> `none`: All public key-type symbols were removed; use a complete metric's string type discriminant.

- `MetricKeyType.FrequencyKeyTypeTypeId` -> `none`: All public key-type symbols were removed; use a complete metric's string type discriminant.

- `MetricKeyType.GaugeKeyTypeTypeId` -> `none`: All public key-type symbols were removed; use a complete metric's string type discriminant.

- `MetricKeyType.HistogramKeyTypeTypeId` -> `none`: All public key-type symbols were removed; use a complete metric's string type discriminant.

- `MetricKeyType.MetricKeyType` -> `Metric.Metric`: Input/state typing and kind configuration now live on the complete Metric\<Input, State\>.

- `MetricKeyType.MetricKeyType.InType` -> `Metric.Metric.Input`: Use Metric.Metric.Input\<M\> to extract a metric's input type.

- `MetricKeyType.MetricKeyType.OutType` -> `Metric.Metric.State`: Use Metric.Metric.State\<M\> to extract a metric's state type.

- `MetricKeyType.MetricKeyType.Untyped` -> `Metric.Metric<any, any>`: The key-type descriptor no longer exists independently of a metric.

- `MetricKeyType.MetricKeyType.Variance` -> `none`: The descriptor variance interface was removed; complete Metric carries Input and State variance.

- `MetricKeyType.MetricKeyTypeTypeId` -> `none`: All public key-type symbols were removed; use a complete metric's string type discriminant.

- `MetricKeyType.SummaryKeyTypeTypeId` -> `none`: All public key-type symbols were removed; use a complete metric's string type discriminant.

- `MetricKeyType.counter` -> `Metric.counter`: The standalone descriptor was folded into the complete Metric.counter constructor.

- `MetricKeyType.frequency` -> `Metric.frequency`: The standalone descriptor was folded into the complete Metric.frequency constructor.

- `MetricKeyType.gauge` -> `Metric.gauge`: The standalone descriptor was folded into the complete Metric.gauge constructor.

- `MetricKeyType.histogram` -> `Metric.histogram`: The standalone descriptor was folded into the complete Metric.histogram constructor.

- `MetricKeyType.isCounterKey` -> `Metric.isMetric + metric.type`: Standalone refinements were removed. Complete metrics expose a Counter, Frequency, Gauge, Histogram, or Summary string discriminant.

- `MetricKeyType.isFrequencyKey` -> `Metric.isMetric + metric.type`: Standalone refinements were removed. Complete metrics expose a Counter, Frequency, Gauge, Histogram, or Summary string discriminant.

- `MetricKeyType.isGaugeKey` -> `Metric.isMetric + metric.type`: Standalone refinements were removed. Complete metrics expose a Counter, Frequency, Gauge, Histogram, or Summary string discriminant.

- `MetricKeyType.isHistogramKey` -> `Metric.isMetric + metric.type`: Standalone refinements were removed. Complete metrics expose a Counter, Frequency, Gauge, Histogram, or Summary string discriminant.

- `MetricKeyType.isMetricKeyType` -> `Metric.isMetric`: Standalone key-type values were removed; test complete metrics instead.

- `MetricKeyType.isSummaryKey` -> `Metric.isMetric + metric.type`: Standalone refinements were removed. Complete metrics expose a Counter, Frequency, Gauge, Histogram, or Summary string discriminant.

- `MetricKeyType.summary` -> `Metric.summary`: The standalone descriptor was folded into the complete Metric.summary constructor.
