# v3 → v4 API map: effect/Effectable

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Effectable`

- `Effectable.ChannelTypeId` -> `Channel.TypeId`: The public channel brand moved to its owning module; v4 uses a string TypeId rather than the v3 Symbol.

- `Effectable.Class` -> `Effectable.Class`: Still available; replace commit() with an asEffect() method returning the Effect. The intermediate v4 override property/getter is no longer supported.

- `Effectable.CommitPrimitive` -> `new<A, E = never, R = never>() => Effect.Effect<A, E, R>`: The named constructor interface was removed; inline the constructor type when needed.

- `Effectable.CommitPrototype` -> `Effectable.Prototype`: Use Effectable.Prototype({ label, evaluate(fiber) { ... } }) and move the old commit body into evaluate.

- `Effectable.EffectPrototype` -> `Effectable.Prototype`: The raw multi-branded prototype was removed; use Prototype with an explicit evaluate callback.

- `Effectable.EffectTypeId` -> `Effect.TypeId`: The public Effect brand moved to Effect; v4 uses a string TypeId rather than the v3 Symbol.

- `Effectable.SinkTypeId` -> `Sink.isSink`: Sink's TypeId is private in v4; use the public guard for runtime checks and public Sink constructors for values.

- `Effectable.StreamTypeId` -> `Stream.TypeId`: The public stream brand moved to its owning module; v4 uses a string TypeId rather than the v3 Symbol.

- `Effectable.StructuralClass` -> `Effectable.Class`: Use Class and migrate commit() to asEffect(); v4 equality is structural by default.

- `Effectable.StructuralCommitPrototype` -> `Effectable.Prototype`: Use Prototype with evaluate; a separate structural prototype is unnecessary because v4 equality is structural by default.
