# v3 → v4 API map: effect/Option

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Option`

- `Option.None` -> `Option.None`: The variant remains, but Option is no longer an Effect or STM subtype.

- `Option.Option` -> `Option.Option`: The union type remains, but Option is no longer an Effect or STM subtype.

- `Option.OptionUnify` -> `Option.OptionUnify`: The unification hook remains under the same name.

- `Option.OptionUnifyIgnore` -> `Option.OptionUnifyIgnore`: The marker remains, without the v3 Effect, Tag, and Either augmentation fields.

- `Option.Some` -> `Option.Some`: The variant remains, but Option is no longer an Effect or STM subtype.

- `Option.TypeId` -> `none`: The v4 Option brand is private and no public TypeId is exported.

- `Option.ap` -> `Option.zipWith`: Use Option.zipWith(self, that, (f, a) =\> f(a)); v4 has no Option.ap.

- `Option.flatMapNullable` -> `Option.flatMapNullishOr`: Renamed with v4 nullish-or terminology.

- `Option.fromNullable` -> `Option.fromNullishOr`: Renamed with v4 nullish-or terminology.

- `Option.getEquivalence` -> `Option.makeEquivalence`: Renamed from getEquivalence to makeEquivalence.

- `Option.getLeft` -> `Option.getFailure`: Either input became Result input, and Left became Failure.

- `Option.getOrder` -> `Option.makeOrder`: Renamed from getOrder to makeOrder.

- `Option.getRight` -> `Option.getSuccess`: Either input became Result input, and Right became Success.

- `Option.liftNullable` -> `Option.liftNullishOr`: Renamed with v4 nullish-or terminology.

- `Option.orElseEither` -> `Option.orElseResult`: Either was replaced by Result; source tracking now uses Failure and Success.

- `Option.partitionMap` -> `Option.partitionMap`: The tuple is now [passes, fails], the reverse of v3 [left, right].
