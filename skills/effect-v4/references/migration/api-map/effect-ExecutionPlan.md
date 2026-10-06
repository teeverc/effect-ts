# v3 → v4 API map: effect/ExecutionPlan

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/ExecutionPlan`

- `ExecutionPlan.ExecutionPlan` -> `ExecutionPlan.ExecutionPlan`: The plan type remains; withRequirements was renamed to captureRequirements.

- `ExecutionPlan.TypesBase` -> `ExecutionPlan.ConfigBase`: The base type for execution-plan step configuration was renamed.

- `ExecutionPlan.make` -> `ExecutionPlan.make`: The variadic execution-plan constructor remains unchanged.
