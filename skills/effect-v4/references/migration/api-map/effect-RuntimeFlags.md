# v3 → v4 API map: effect/RuntimeFlags

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/RuntimeFlags`

- `RuntimeFlags.CooperativeYielding` -> `References.PreventSchedulerYield`: Use the scheduler Reference with inverse boolean meaning.

- `RuntimeFlags.Interruption` -> `Effect.interruptible | Effect.uninterruptible`: The bit flag was removed; control interruptibility with Effect regions.

- `RuntimeFlags.None` -> `none`: The empty runtime-flags value and its type were removed.

- `RuntimeFlags.OpSupervision` -> `none`: Operation supervision and its runtime flag were removed.

- `RuntimeFlags.RuntimeFlag` -> `none`: Individual bit flags were removed; use the corresponding semantic API.

- `RuntimeFlags.RuntimeFlags` -> `none`: The aggregate runtime-flags bitset was removed.

- `RuntimeFlags.RuntimeMetrics` -> `Metric.FiberRuntimeMetrics`: Runtime metrics are now configured through a Context.Reference service rather than a bit flag.

- `RuntimeFlags.WindDown` -> `none`: The wind-down flag is no longer public.

- `RuntimeFlags.cooperativeYielding` -> `!References.PreventSchedulerYield`: Read the scheduler Reference and negate it; the aggregate flags value was removed.

- `RuntimeFlags.diff` -> `none`: The runtime-flags bitset was removed; configure each semantic behavior directly.

- `RuntimeFlags.differ` -> `none`: The runtime-flags bitset and patch differ were removed.

- `RuntimeFlags.disable` -> `none`: The generic flag operation was removed; disable the corresponding behavior directly.

- `RuntimeFlags.disableAll` -> `none`: The aggregate flags value was removed; configure scheduler yielding, interruptibility, and metrics independently.

- `RuntimeFlags.disableCooperativeYielding` -> `Effect.provideService(References.PreventSchedulerYield, true)`: Prevent scheduler yielding through its Context.Reference.

- `RuntimeFlags.disableInterruption` -> `Effect.uninterruptible`: Use an uninterruptible region instead of changing a runtime flag.

- `RuntimeFlags.disableOpSupervision` -> `none`: Operation supervision and its runtime flag were removed.

- `RuntimeFlags.disableRuntimeMetrics` -> `Metric.disableRuntimeMetrics`: Disable fiber runtime metrics directly; use disableRuntimeMetricsLayer when providing a Layer.

- `RuntimeFlags.disableWindDown` -> `none`: The wind-down flag is runtime-internal in v4; use normal scoped finalizers and explicit interruptibility regions.

- `RuntimeFlags.enable` -> `none`: The generic flag operation was removed; enable the corresponding behavior directly.

- `RuntimeFlags.enableAll` -> `none`: The aggregate flags value was removed; configure scheduler yielding, interruptibility, and metrics independently.

- `RuntimeFlags.enableCooperativeYielding` -> `Effect.provideService(References.PreventSchedulerYield, false)`: Allow scheduler yielding through its Context.Reference.

- `RuntimeFlags.enableInterruption` -> `Effect.interruptible`: Use an interruptible region instead of changing a runtime flag.

- `RuntimeFlags.enableOpSupervision` -> `none`: Operation supervision and its runtime flag were removed.

- `RuntimeFlags.enableRuntimeMetrics` -> `Metric.enableRuntimeMetrics`: Enable fiber runtime metrics directly; use enableRuntimeMetricsLayer when providing a Layer.

- `RuntimeFlags.enableWindDown` -> `none`: The wind-down flag is runtime-internal in v4; use normal scoped finalizers and explicit interruptibility regions.

- `RuntimeFlags.interruptible` -> `none`: There is no public current-interruptibility getter; structure the program with Effect.interruptible or Effect.uninterruptible.

- `RuntimeFlags.interruption` -> `none`: Interruptibility is controlled by Effect regions rather than queried from a flags value.

- `RuntimeFlags.isDisabled` -> `none`: There is no aggregate flags value to query; inspect or control the corresponding semantic facility.

- `RuntimeFlags.make` -> `none`: The runtime-flags bitset was removed; do not recreate it in v4.

- `RuntimeFlags.none` -> `none`: The runtime-flags bitset was removed; configure each semantic behavior independently.

- `RuntimeFlags.opSupervision` -> `none`: Operation supervision and its runtime flag were removed.

- `RuntimeFlags.patch` -> `none`: Aggregate runtime-flags patches were removed; configure each semantic behavior directly.

- `RuntimeFlags.render` -> `none`: The runtime-flags bitset and its renderer were removed.

- `RuntimeFlags.runtimeMetrics` -> `Metric.FiberRuntimeMetrics`: Read the Context.Reference and test for undefined instead of querying a bit flag.

- `RuntimeFlags.toSet` -> `none`: The runtime-flags bitset was removed; there is no set conversion.

- `RuntimeFlags.windDown` -> `none`: The wind-down flag is no longer public.
