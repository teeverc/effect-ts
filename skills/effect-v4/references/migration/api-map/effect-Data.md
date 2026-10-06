# v3 → v4 API map: effect/Data

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Data`

- `Data.Case` -> `Data.TaggedEnum.ConstructorFrom`: The Case namespace was removed; its constructor helper moved under TaggedEnum.

- `Data.Case.Constructor` -> `Data.TaggedEnum.ConstructorFrom`: Use the v4 tagged-enum constructor-function type.

- `Data.Structural` -> `Data.Class`: Extend Data.Class instead of the removed Structural constructor alias.

- `Data.TaggedEnum` -> `Data.TaggedEnum`: Still exported with the same record-to-discriminated-union role.

- `Data.TaggedEnum.GenericMatchers` -> `Data.TaggedEnum.GenericMatchers`: Still exported with $is and $match helpers.

- `Data.array` -> `none`: Use a normal copied array such as [...values]; v4 compares plain arrays structurally.

- `Data.case` -> `none`: Use an ordinary typed object or identity constructor; plain objects are structurally equal in v4.

- `Data.struct` -> `none`: Use an ordinary object or {...value}; plain objects are structurally equal in v4.

- `Data.tuple` -> `none`: Use a normal tuple literal; plain tuples are structurally equal in v4.

- `Data.unsafeArray` -> `none`: Use the array directly; v4 no longer needs prototype mutation for structural equality.

- `Data.unsafeStruct` -> `none`: Use the object directly; v4 no longer needs prototype mutation for structural equality.
