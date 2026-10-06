# v3 → v4 API map: effect/Pretty

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Pretty`

- `Pretty.Pretty` -> `Formatter.Formatter`: The formatter function type is now exported by Formatter.

- `Pretty.PrettyAnnotation` -> `Schema.Annotations.ToFormatter.Declaration`: Custom declaration formatter annotations now use the toFormatter key in Schema.Annotations.

#### `Pretty.make`

**Replacement:** `Schema.toFormatter`

Formatter derivation moved onto Schema.

**Example**

```ts
Schema.toFormatter(schema)
```

- `Pretty.match` -> `Schema.toFormatter`: The compiler match table was removed; customize traversal with the toFormatter onBefore option.
