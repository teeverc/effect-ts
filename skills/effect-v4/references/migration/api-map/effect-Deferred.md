# v3 → v4 API map: effect/Deferred

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Deferred`

- `Deferred.Deferred` -> `Deferred.Deferred`: The model remains but is now Pipeable rather than an Effect subtype; replace yielding the Deferred itself with Deferred.await.

- `Deferred.Deferred.Variance` -> `Deferred.Deferred.Variance`: The marker remains under Deferred.Deferred, but its brand uses an internal type id; ordinary code should use Deferred.Deferred directly.

- `Deferred.DeferredTypeId` -> `none`: The Deferred type id is internal in v4; do not inspect or construct the brand directly.

- `Deferred.DeferredUnify` -> `none`: Deferred is no longer an Effect subtype, so its Effect unification helper was removed; call Deferred.await explicitly.

- `Deferred.DeferredUnifyIgnore` -> `none`: Deferred is no longer an Effect subtype, so its Effect unification ignore marker was removed.

- `Deferred.await` -> `Deferred.await`: The function remains; call it explicitly because Deferred is no longer an Effect subtype in v4.

- `Deferred.makeAs` -> `Deferred.makeUnsafe`: Use the synchronous v4 constructor; it no longer accepts or records a FiberId.

- `Deferred.poll` -> `Deferred.poll`: The function remains and returns an Option containing the stored completion Effect.

- `Deferred.unsafeDone` -> `Deferred.doneUnsafe`: The unsafe suffix moved to the end; the v4 function returns whether this call completed the Deferred.

- `Deferred.unsafeMake` -> `Deferred.makeUnsafe`: The unsafe suffix moved to the end, and the v4 constructor takes no FiberId argument.
