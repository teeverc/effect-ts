# v3 → v4 API map: effect/MetricState

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MetricState`

- `MetricState.CounterStateTypeId` -> `none`: All state brand symbols were removed; v4 state interfaces are structural.

- `MetricState.FrequencyStateTypeId` -> `none`: All state brand symbols were removed; v4 state interfaces are structural.

- `MetricState.GaugeStateTypeId` -> `none`: All state brand symbols were removed; v4 state interfaces are structural.

- `MetricState.HistogramStateTypeId` -> `none`: All state brand symbols were removed; v4 state interfaces are structural.

- `MetricState.MetricState` -> `Metric.Metric.State`: The common branded state model was removed; extract a complete metric's state with Metric.Metric.State\<M\> or use a concrete state interface.

- `MetricState.MetricState.Untyped` -> `Metric.Metric.Snapshot['state']`: Use the state union from Metric.Metric.Snapshot, or explicitly union the five structural state interfaces.

- `MetricState.MetricState.Variance` -> `none`: States are structural objects and no longer carry a variance brand.

- `MetricState.MetricStateTypeId` -> `none`: All state brand symbols were removed; v4 state interfaces are structural.

- `MetricState.SummaryStateTypeId` -> `none`: All state brand symbols were removed; v4 state interfaces are structural.

- `MetricState.counter` -> `Metric.CounterState`: There is no state constructor. Obtain the structural state with Metric.value(Metric.counter(...)).

- `MetricState.frequency` -> `Metric.FrequencyState`: There is no state constructor. Obtain the structural state with Metric.value(Metric.frequency(...)).

- `MetricState.gauge` -> `Metric.GaugeState`: There is no state constructor. Obtain the structural state with Metric.value(Metric.gauge(...)).

- `MetricState.histogram` -> `Metric.HistogramState`: There is no state constructor. Obtain the structural state with Metric.value(Metric.histogram(...)).

- `MetricState.isCounterState` -> `none`: Standalone state guards were removed. Retain the enclosing snapshot and switch on snapshot.type for runtime discrimination.

- `MetricState.isFrequencyState` -> `none`: Standalone state guards were removed. Retain the enclosing snapshot and switch on snapshot.type for runtime discrimination.

- `MetricState.isGaugeState` -> `none`: Standalone state guards were removed. Retain the enclosing snapshot and switch on snapshot.type for runtime discrimination.

- `MetricState.isHistogramState` -> `none`: Standalone state guards were removed. Retain the enclosing snapshot and switch on snapshot.type for runtime discrimination.

- `MetricState.isMetricState` -> `none`: Standalone state guards were removed. Retain the enclosing snapshot and switch on snapshot.type for runtime discrimination.

- `MetricState.isSummaryState` -> `none`: Standalone state guards were removed. Retain the enclosing snapshot and switch on snapshot.type for runtime discrimination.

- `MetricState.summary` -> `Metric.SummaryState`: There is no state constructor. Obtain the structural state with Metric.value(Metric.summary(...)).
