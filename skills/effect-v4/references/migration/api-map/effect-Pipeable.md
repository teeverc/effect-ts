# v3 → v4 API map: effect/Pipeable

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Pipeable`

- `Pipeable.PipeableConstructor` -> `Pipeable.PipeableConstructor`: Still exported; its rest arguments are ReadonlyArray\<any\> in v4, so make explicit constructor typings readonly-compatible.
