# v3 → v4 API map: effect/Ref

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Ref`

- `Ref.Ref` -> `Ref.Ref`: The model remains but is now Pipeable rather than an Effect or Readable subtype; read it explicitly with Ref.get.

- `Ref.Ref.Variance` -> `Ref.Ref.Variance`: The marker remains under Ref.Ref, but its brand uses an internal type id; ordinary code should use Ref.Ref directly.

- `Ref.RefTypeId` -> `none`: The Ref type id is internal in v4; do not inspect or construct the brand directly.

- `Ref.RefUnify` -> `none`: Ref is no longer an Effect subtype, so its Effect unification helper was removed; call Ref.get explicitly.

- `Ref.RefUnifyIgnore` -> `none`: Ref is no longer an Effect subtype, so its Effect unification ignore marker was removed.

- `Ref.getAndSet` -> `Ref.getAndSet`: The operation remains with data-first and data-last forms.

- `Ref.getAndUpdate` -> `Ref.getAndUpdate`: The operation remains with data-first and data-last forms.

- `Ref.getAndUpdateSome` -> `Ref.getAndUpdateSome`: The operation remains; Option.none leaves the value unchanged.

- `Ref.modify` -> `Ref.modify`: The operation remains with data-first and data-last forms.

- `Ref.set` -> `Ref.set`: The operation remains with data-first and data-last forms.

- `Ref.setAndGet` -> `Ref.setAndGet`: The operation remains with data-first and data-last forms.

- `Ref.unsafeMake` -> `Ref.makeUnsafe`: The unsafe suffix moved to the end.

- `Ref.update` -> `Ref.update`: The operation remains with data-first and data-last forms.

- `Ref.updateAndGet` -> `Ref.updateAndGet`: The operation remains with data-first and data-last forms.

- `Ref.updateSome` -> `Ref.updateSome`: The operation remains; Option.none leaves the value unchanged.

- `Ref.updateSomeAndGet` -> `Ref.updateSomeAndGet`: The operation remains; Option.none leaves the value unchanged and returns the current value.
