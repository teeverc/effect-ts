# v3 → v4 API map: effect/SynchronizedRef

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/SynchronizedRef`

- `SynchronizedRef.SynchronizedRef` -> `SynchronizedRef.SynchronizedRef`: The model remains but no longer extends Ref; read and update it through explicit SynchronizedRef operations. The curried v4 modifySomeEffect takes only the callback, which returns an Effect of [result, Option\<newValue\>]; remove the v3 fallback and outer Option.

- `SynchronizedRef.SynchronizedRef.Variance` -> `none`: The public nested variance marker was removed. SynchronizedRef uses an internal brand in v4, so do not refer to a variance interface directly.

- `SynchronizedRef.SynchronizedRefTypeId` -> `none`: The SynchronizedRef type id is internal in v4; do not inspect or construct the brand directly.

- `SynchronizedRef.SynchronizedRefUnify` -> `none`: SynchronizedRef is no longer an Effect subtype, so its Effect unification helper was removed; call SynchronizedRef.get explicitly.

- `SynchronizedRef.SynchronizedRefUnifyIgnore` -> `none`: SynchronizedRef is no longer an Effect subtype, so its Effect unification ignore marker was removed.

- `SynchronizedRef.unsafeMake` -> `SynchronizedRef.makeUnsafe`: The unsafe suffix moved to the end.
