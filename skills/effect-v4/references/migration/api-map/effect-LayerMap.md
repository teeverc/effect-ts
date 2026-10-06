# v3 → v4 API map: effect/LayerMap

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/LayerMap`

- `LayerMap.LayerMap` -> `LayerMap.LayerMap`: The type remains; runtime(key) became contextEffect(key) and returns Context.

- `LayerMap.Service` -> `LayerMap.Service`: Use layer instead of Default, layerNoDeps instead of DefaultWithoutDependencies, and contextEffect instead of runtime. Preloading does not remove acquisition errors from later lookups, which can reacquire expired or invalidated entries.

- `LayerMap.Service.Context` -> `LayerMap.Service.Services<Options>`: The input-services extractor was renamed.

- `LayerMap.TagClass` -> `LayerMap.TagClass`: The type remains and now extends Context.ServiceClass; use the renamed layer and contextEffect members.

- `LayerMap.TypeId` -> `none`: The LayerMap marker is private in v4 and no public guard exists.
