# v3 → v4 API map: effect/MetricBoundaries

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MetricBoundaries`

- `MetricBoundaries.MetricBoundaries` -> `ReadonlyArray<number>`: The wrapper was removed; Metric.histogram accepts plain boundaries in its options.

- `MetricBoundaries.MetricBoundariesTypeId` -> `none`: Boundaries are unbranded arrays, so the guard and public type-id symbol have no replacement.

- `MetricBoundaries.exponential` -> `Metric.exponentialBoundaries`: Moved into effect/Metric and now returns ReadonlyArray\<number\>. V4 also filters non-positive boundaries.

- `MetricBoundaries.fromIterable` -> `Metric.boundariesFromIterable`: Moved into effect/Metric and now returns an unbranded ReadonlyArray\<number\>; v4 removes non-positive values before appending Infinity.

- `MetricBoundaries.isMetricBoundaries` -> `none`: Boundaries are unbranded arrays, so the guard and public type-id symbol have no replacement.

- `MetricBoundaries.linear` -> `Metric.linearBoundaries`: Moved into effect/Metric, but the compared v4 implementation uses start + i + width rather than v3's start + i \* width. Preserve the v3 formula manually when width is not 1.
