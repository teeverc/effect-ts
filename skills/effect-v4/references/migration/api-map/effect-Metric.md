# v3 → v4 API map: effect/Metric

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Metric`

- `Metric.Metric` -> `Metric.Metric<Input, State>`: Drop the v3 key-type parameter. Metrics are no longer callable; use Effect.trackSuccesses for instrumentation and Metric.update or Metric.value for operations.

- `Metric.Metric.Variance` -> `none`: The public variance interface was removed; Metric\<Input, State\> carries variance markers directly.

- `Metric.MetricApply` -> `none`: Removed with Metric.make; v4 has no public low-level custom-metric constructor type.

- `Metric.MetricTypeId` -> `Metric.isMetric`: The public unique-symbol type id was removed; use Metric.isMetric for runtime refinement.

- `Metric.fiberActive` -> `Metric.enableRuntimeMetrics + Metric.snapshot`: The concrete metric is no longer exported. Enable runtime metrics, then read the Gauge snapshot whose id is child\_fibers\_active.

- `Metric.fiberFailures` -> `Metric.enableRuntimeMetrics + Metric.snapshot`: The concrete metric is no longer exported. Enable runtime metrics, then read the Counter snapshot whose id is child\_fiber\_failures.

- `Metric.fiberLifetimes` -> `none`: The built-in lifetime histogram was removed. Define a Metric.timer and instrument selected effects with Effect.trackDuration when lifetime data is required.

- `Metric.fiberStarted` -> `Metric.enableRuntimeMetrics + Metric.snapshot`: The concrete metric is no longer exported. Enable runtime metrics, then read the Counter snapshot whose id is child\_fibers\_started.

- `Metric.fiberSuccesses` -> `Metric.enableRuntimeMetrics + Metric.snapshot`: The concrete metric is no longer exported. Enable runtime metrics, then read the Counter snapshot whose id is child\_fiber\_successes.

- `Metric.fromMetricKey` -> `Metric.counter / Metric.gauge / Metric.frequency / Metric.histogram / Metric.summary`: MetricKey and MetricKeyType were removed. Construct the required primitive metric directly.

- `Metric.globalMetricRegistry` -> `Metric.MetricRegistry`: The process-global registry became a Context.Reference whose service is a Map. Access it in context or provide a fresh Map for isolation; use Metric.snapshot for normal reads.

- `Metric.increment` -> `Metric.update / Metric.modify`: Use Metric.update(counter, 1 or 1n) for counters and Metric.modify(gauge, 1 or 1n) for gauges; gauge update sets an absolute value while modify adds a delta.

- `Metric.incrementBy` -> `Metric.update / Metric.modify`: Use Metric.update(counter, amount) for counters and Metric.modify(gauge, amount) for gauges.

- `Metric.make` -> `none`: The low-level arbitrary metric constructor was removed. Use a public primitive constructor and compose with mapInput, withConstantInput, and withAttributes.

- `Metric.map` -> `Metric.value + Effect.map`: Metric-level state mapping was removed. Transform a read with Effect.map(Metric.value(metric), f).

- `Metric.mapType` -> `none`: Drop this call. V4 Metric has no key-type type parameter and exposes a fixed runtime type discriminator.

- `Metric.set` -> `Metric.update`: Use Metric.update(gauge, value); v4 update replaces a gauge's current value.

- `Metric.succeed` -> `none`: Constant synthetic metrics were removed. Keep constants outside the metric and use Effect.succeed when an Effect value is required.

- `Metric.summaryTimestamp` -> `Metric.summaryWithTimestamp`: Renamed and called as Metric.summaryWithTimestamp(name, options). Remove the v3 error option; inputs remain value/timestamp pairs.

- `Metric.sync` -> `none`: Lazy synthetic metrics were removed. Keep the computation outside the metric and use Effect.sync when an Effect value is required.

- `Metric.tagged` -> `Metric.withAttributes`: Replace tags with attributes, for example Metric.withAttributes(metric, { [key]: value }).

- `Metric.taggedWithLabels` -> `Metric.withAttributes`: Replace MetricLabel objects with a string record or array of string tuples passed to Metric.withAttributes.

- `Metric.taggedWithLabelsInput` -> `Metric.withAttributes + Metric.update`: No dynamic-attribute transform remains. Compute attributes at each update or tracking site, wrap with Metric.withAttributes, then update the metric.

- `Metric.timerWithBoundaries` -> `Metric.timer`: Use Metric.timer(name, { boundaries, description }); boundaries moved into the options object.

- `Metric.trackAll` -> `Effect.track`: Moved to Effect; use effect.pipe(Effect.track(metric, () =\> input)).

- `Metric.trackDefect` -> `Effect.trackDefects`: Moved to Effect; use effect.pipe(Effect.trackDefects(metric)).

- `Metric.trackDefectWith` -> `Effect.trackDefects`: Moved to Effect; pass the mapper as the optional second argument.

- `Metric.trackDurationWith` -> `Effect.trackDuration`: Moved to Effect; pass the mapper as the optional second argument. V4 records duration on every Exit, whereas v3 updated only after success.

- `Metric.trackError` -> `Effect.trackErrors`: Moved to Effect; use effect.pipe(Effect.trackErrors(metric)).

- `Metric.trackErrorWith` -> `Effect.trackErrors`: Moved to Effect; pass the mapper as the optional second argument.

- `Metric.trackSuccess` -> `Effect.trackSuccesses`: Moved to Effect; use effect.pipe(Effect.trackSuccesses(metric)).

- `Metric.trackSuccessWith` -> `Effect.trackSuccesses`: Moved to Effect; pass the mapper as the optional second argument.

- `Metric.unsafeSnapshot` -> `Metric.snapshotUnsafe`: Renamed and now requires an explicit Context.Context\<never\>. It returns structural snapshots rather than MetricPair values.

- `Metric.withNow` -> `Metric.summary`: Metric.summary reads the current Clock automatically; use Metric.summaryWithTimestamp when timestamps are supplied explicitly. The generic timestamp-injecting combinator was removed.

- `Metric.zip` -> `Effect.all + Metric.update / Metric.value`: Composite metrics were removed. Use Effect.all to update both metrics or combine their Metric.value reads.
