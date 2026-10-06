# v3 → v4 API map: effect/Function

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Function`

- `Function.FunctionN` -> `Function.FunctionN`: No call-site migration; v4 keeps the same function shape as a type alias.

- `Function.LazyArg` -> `Function.LazyArg`: No call-site migration; v4 keeps the same lazy function shape as a type alias.

- `Function.isFunction` -> `Predicate.isFunction`: The function refinement moved to Predicate.

- `Function.unsafeCoerce` -> `Function.cast`: Renamed type-only cast; runtime behavior remains identity with no validation.
