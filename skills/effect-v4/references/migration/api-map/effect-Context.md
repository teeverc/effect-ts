# v3 → v4 API map: effect/Context

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Context`

- `Context.Context` -> `Context.Context<Services>`: The type remains; unsafeMap is now mapUnsafe and v4 also exposes mutable.

- `Context.GenericTag` -> `Context.Service<T>(id)`: Use the function-style Context.Service constructor.

- `Context.ReadonlyTag` -> `Context.Key<Identifier, Shape>`: Use the renamed service-key interface.

- `Context.Reference` -> `Context.Reference<Shape>`: Use Context.Reference\<Shape\>(id, { defaultValue }); the identifier type parameter was removed.

- `Context.ReferenceClass` -> `Context.Reference<Shape>(id, { defaultValue })`: Replace reference subclasses with a constant created by Context.Reference.

- `Context.ReferenceTypeId` -> `none`: The marker is private in v4; use Context.isReference for runtime discrimination.

- `Context.Tag` -> `Context.Service`: Use Context.Service\<T\>(id), or Context.Service\<Self, Shape\>()(id) for class syntax.

- `Context.Tag.Service` -> `Context.Service.Shape<T>`: The namespace type helper was renamed with Tag.

- `Context.TagClass` -> `Context.ServiceClass<Self, Id, Shape>`: Use the renamed class-style service-key type.

- `Context.TagClassShape` -> `Context.ServiceClass.Shape<Id, Shape>`: Use the renamed namespace type helper.

- `Context.TagTypeId` -> `Context.ServiceTypeId`: The public type identifier was renamed with Tag.

- `Context.TagUnify` -> `none`: The Context-specific unification hook was removed; Context.Key already extends Effect.

- `Context.TagUnifyIgnore` -> `none`: The Context-specific Unify-ignore artifact was removed.

- `Context.TypeId` -> `none`: The Context marker is private in v4; use Context.isContext for runtime checks.

- `Context.ValidTagsById` -> `<I extends R, S>(key: Context.Key<I, S>)`: The alias was removed; express the Context.Key constraint directly.

- `Context.isTag` -> `Context.isKey`: The service-key guard was renamed.

- `Context.unsafeGet` -> `Context.getUnsafe`: The unsafe getter was renamed.

- `Context.unsafeMake` -> `Context.makeUnsafe`: The unsafe constructor was renamed and accepts a ReadonlyMap.
