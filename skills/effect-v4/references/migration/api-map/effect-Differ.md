# v3 → v4 API map: effect/Differ

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Differ`

- `Differ.Differ` -> `Differ.Differ`: The interface remains, but is now an unbranded structural interface and patch takes arguments as patch(oldValue, patch).

- `Differ.Differ.Chunk` -> `JsonPatch.JsonPatch`: The Chunk patch namespace was removed; Schema.toDifferJsonPatch uses the common RFC 6902 patch representation.

- `Differ.Differ.Chunk.Patch` -> `JsonPatch.JsonPatch`: Use the patch type returned by Schema.toDifferJsonPatch instead of the removed Chunk-specific patch type.

- `Differ.Differ.Chunk.TypeId` -> `none`: Chunk-specific patches and their public brand were removed; do not inspect a patch type id.

- `Differ.Differ.Context` -> `Context.Context`: The Context patch namespace was removed; construct and merge Context values explicitly rather than diffing environments.

- `Differ.Differ.Context.Patch` -> `none`: Context patches were removed; use Context.add, Context.merge, and Context.omit to build the desired Context directly.

- `Differ.Differ.Context.TypeId` -> `none`: Context patches and their public brand were removed.

- `Differ.Differ.HashMap` -> `JsonPatch.JsonPatch`: The HashMap patch namespace was removed; derive a JSON Patch differ from a Schema for the complete value.

- `Differ.Differ.HashMap.Patch` -> `JsonPatch.JsonPatch`: Use the patch type returned by Schema.toDifferJsonPatch instead of the removed HashMap-specific patch type.

- `Differ.Differ.HashMap.TypeId` -> `none`: HashMap-specific patches and their public brand were removed; do not inspect a patch type id.

- `Differ.Differ.HashSet.Patch` -> `JsonPatch.JsonPatch`: Use the patch type returned by Schema.toDifferJsonPatch instead of the removed HashSet-specific patch type.

- `Differ.Differ.HashSet.TypeId` -> `none`: HashSet-specific patches and their public brand were removed; do not inspect a patch type id.

- `Differ.Differ.Or` -> `JsonPatch.JsonPatch`: The Either patch namespace was removed; derive one JSON Patch differ from the Schema for the union value.

- `Differ.Differ.Or.Patch` -> `JsonPatch.JsonPatch`: Use the patch type returned by Schema.toDifferJsonPatch instead of the removed Either-specific patch type.

- `Differ.Differ.Or.TypeId` -> `none`: Either-specific patches and their public brand were removed; do not inspect a patch type id.

- `Differ.Differ.ReadonlyArray` -> `JsonPatch.JsonPatch`: The ReadonlyArray patch namespace was removed; Schema.toDifferJsonPatch uses the common RFC 6902 patch representation.

- `Differ.Differ.ReadonlyArray.Patch` -> `JsonPatch.JsonPatch`: Use the patch type returned by Schema.toDifferJsonPatch instead of the removed ReadonlyArray-specific patch type.

- `Differ.Differ.ReadonlyArray.TypeId` -> `none`: ReadonlyArray-specific patches and their public brand were removed; do not inspect a patch type id.

- `Differ.TypeId` -> `none`: Differ is an unbranded structural interface in v4; do not inspect or implement a public type id.

- `Differ.chunk` -> `Schema.toDifferJsonPatch`: Derive a JSON Patch differ from a Schema for the whole Chunk; v4 no longer exposes collection-specific patch constructors.

- `Differ.combine` -> `differ.combine`: Call the combine method on the Differ value directly; the standalone helper was removed.

- `Differ.diff` -> `differ.diff`: Call the diff method on the Differ value directly; the standalone helper was removed.

- `Differ.empty` -> `differ.empty`: Read the empty property from the Differ value directly; the standalone accessor was removed.

- `Differ.environment` -> `none`: The Context differ was removed; construct the target Context explicitly with Context.add, Context.merge, and Context.omit.

- `Differ.hashMap` -> `Schema.toDifferJsonPatch`: Derive a JSON Patch differ from a Schema for the whole map; v4 no longer exposes collection-specific patch constructors.

- `Differ.hashSet` -> `Schema.toDifferJsonPatch`: Derive a JSON Patch differ from a Schema for the whole set; v4 no longer exposes collection-specific patch constructors.

- `Differ.make` -> `object literal satisfying Differ.Differ`: Differ is structural in v4; provide empty, diff, combine, and patch methods directly, with patch(oldValue, patch) argument order.

- `Differ.orElseEither` -> `Schema.toDifferJsonPatch`: Derive one differ from the Schema for the Either value; the compositional Either-specific differ and patch type were removed.

- `Differ.patch` -> `differ.patch`: Call the method directly and reverse the v3 method order: differ.patch(oldValue, patch).

- `Differ.readonlyArray` -> `Schema.toDifferJsonPatch`: Derive a JSON Patch differ from a Schema for the whole array; v4 no longer exposes collection-specific patch constructors.

- `Differ.transform` -> `object literal satisfying Differ.Differ`: There is no transform combinator; define a structural Differ that maps values before delegating to the original differ.

- `Differ.update` -> `object literal satisfying Differ.Differ`: The update constructor was removed; define empty, diff, combine, and patch directly for function patches, or use Schema.toDifferJsonPatch.

- `Differ.updateWith` -> `object literal satisfying Differ.Differ`: The updateWith constructor was removed; encode the desired merge rule in a structural Differ implementation.
