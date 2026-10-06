# v3 → v4 API map: effect/JSONSchema

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/JSONSchema`

- `JSONSchema.JsonSchema7` -> `JsonSchema.JsonSchema`: The draft-07-specific union was replaced by the dialect-neutral JSON Schema model.

- `JSONSchema.JsonSchema7Any` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7AnyOf` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Array` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Boolean` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Enum` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Enums` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Integer` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Never` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Null` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Number` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Numeric` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Object` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Ref` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Root` -> `JsonSchema.Document<"draft-07">`: Use a typed JSON Schema document for a draft-07 root and definitions.

- `JSONSchema.JsonSchema7String` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Unknown` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7Void` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7empty` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchema7object` -> `JsonSchema.JsonSchema`: Individual draft-07 node interfaces were consolidated into JsonSchema.JsonSchema.

- `JSONSchema.JsonSchemaAnnotations` -> `Schema.Annotations.Documentation`: Schema metadata now uses string-keyed Schema annotations; JSON Schema-specific checks use toJsonSchema annotations.

#### `JSONSchema.fromAST`

**Replacement:** `Schema.toJsonSchemaDocument`

Wrap a low-level AST with Schema.make, then generate a draft 2020-12 document. Generation defaults to onExcessProperty: 'ignore'; use 'error' for closed objects. JSON Schema validation is approximate and does not replace Effect decoding.

**Example**

```ts
Schema.toJsonSchemaDocument(Schema.make(ast))
```

#### `JSONSchema.make`

**Replacement:** `Schema.toJsonSchemaDocument`

Generate draft 2020-12, then call JsonSchema.toDocumentDraft07 when draft-07 output is required. Generation now defaults to onExcessProperty: 'ignore'; pass 'error' to retain closed-object output, and use the same option when decoding to reject excess properties. The earlier v4 additionalProperties generation option was removed. JSON Schema validation is approximate and does not replace Effect decoding.

**Example**

```ts
JsonSchema.toDocumentDraft07(Schema.toJsonSchemaDocument(schema))
```
