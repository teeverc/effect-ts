# v3 → v4 API map: effect/MetricHook

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MetricHook`

- `MetricHook.MetricHook` -> `Metric.Metric.Hooks`: The closest public structural interface is Metric.Metric.Hooks\<Input, State\>; get, update, and modify also receive a Context.

- `MetricHook.MetricHook.Root` -> `Metric.Metric.Hooks<any, any>`: The named aliases were removed; specialize the public Hooks interface directly when low-level typing is unavoidable.

- `MetricHook.MetricHook.Untyped` -> `Metric.Metric.Hooks<any, any>`: The named aliases were removed; specialize the public Hooks interface directly when low-level typing is unavoidable.

- `MetricHook.MetricHook.Variance` -> `none`: Hooks are structural and unbranded; the variance helper and public symbol were removed.

- `MetricHook.MetricHookTypeId` -> `none`: Hooks are structural and unbranded; the variance helper and public symbol were removed.

- `MetricHook.counter` -> `Metric.counter`: Hook construction was folded into the complete Metric.counter constructor; hooks are internal.

- `MetricHook.frequency` -> `Metric.frequency`: Hook construction was folded into the complete Metric.frequency constructor; hooks are internal.

- `MetricHook.gauge` -> `Metric.gauge`: Hook construction was folded into the complete Metric.gauge constructor; hooks are internal.

- `MetricHook.histogram` -> `Metric.histogram`: Hook construction was folded into the complete Metric.histogram constructor; hooks are internal.

- `MetricHook.make` -> `none`: There is no public hook constructor; metric classes create and attach hooks internally.

- `MetricHook.onModify` -> `none`: The operation-specific hook decorators were removed. Metric.mapInput cannot distinguish update from modify.

- `MetricHook.onUpdate` -> `none`: The operation-specific hook decorators were removed. Metric.mapInput cannot distinguish update from modify.

- `MetricHook.summary` -> `Metric.summary`: Hook construction was folded into the complete Metric.summary constructor; hooks are internal.
