# v3 → v4 API map: effect/Arbitrary

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Arbitrary`

- `Arbitrary.ArbitraryAnnotation` -> `Schema.Annotations.ToArbitrary.Declaration`: Arbitrary derivation annotations now live in Schema.Annotations. Attach a toCodecArbitrary declaration callback that returns a Schema Link.

- `Arbitrary.ArbitraryGenerationContext` -> `Schema.Annotations.ToArbitrary.DeclarationInput`: Native arbitrary callbacks receive DeclarationInput with decoded type-parameter schemas and normalized constraints.

- `Arbitrary.LazyArbitrary` -> `effect/Arbitrary#Arbitrary`: The generated-value description is now the native Arbitrary interface from effect/Arbitrary.

#### `Arbitrary.make`

**Replacement:** `effect/Arbitrary#schema`

Derive a native Arbitrary from a Schema. Effect no longer bridges to fast-check.

**Example**

```ts
Arbitrary.schema(schema)
```

#### `Arbitrary.makeLazy`

**Replacement:** `effect/Arbitrary#schema`

Lazy and eager Schema derivation are the same native Arbitrary.schema constructor.

**Example**

```ts
Arbitrary.schema(schema)
```
