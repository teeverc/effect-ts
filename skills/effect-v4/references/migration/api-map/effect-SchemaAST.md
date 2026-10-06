# v3 → v4 API map: effect/SchemaAST

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/SchemaAST`

- `SchemaAST.AST` -> `SchemaAST.AST`: The name remains, but its constructor and fields changed in the v4 Base/check/context/encoding model.

- `SchemaAST.Annotated` -> `SchemaAST.AST`: The public base type was removed. Use the AST union; every variant still exposes annotations, checks, encoding, and context.

- `SchemaAST.AnyKeyword` -> `SchemaAST.Any`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.ArbitraryAnnotationId` -> `Schema.Annotations.ToArbitrary`: Symbol annotation IDs were removed. Declarations use the toCodecArbitrary annotation; filters use arbitraryConstraint.

- `SchemaAST.BatchingAnnotation` -> `none`: Per-schema batching annotations were removed. They have no direct replacement. ParseOptions.concurrency controls product child parsing only; configure request batching separately.

- `SchemaAST.BatchingAnnotationId` -> `none`: Symbol annotation IDs were removed and batching is no longer a schema annotation.

- `SchemaAST.BigIntKeyword` -> `SchemaAST.BigInt`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.BooleanKeyword` -> `SchemaAST.Boolean`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.BrandAnnotation` -> `none`: Runtime brand annotations were removed. Schema.brand adds a TypeScript-only nominal distinction and does not store brand metadata in the AST.

- `SchemaAST.BrandAnnotationId` -> `Schema.brand`: Use Schema.brand for a TypeScript-only nominal distinction. V4 does not store brand metadata in the AST.

- `SchemaAST.Compiler` -> `none`: The generic AST compiler abstraction was removed; traverse the discriminated SchemaAST.AST union directly or use a higher-level Schema derivation API.

- `SchemaAST.ComposeTransformation` -> `SchemaAST.Encoding`: The marker transformation was replaced by explicit SchemaAST.Link encoding chains.

- `SchemaAST.ConcurrencyAnnotation` -> `SchemaAST.ParseOptions`: Per-schema concurrency annotations were removed. Pass concurrency in runtime ParseOptions when creating or calling a decoder, encoder, or constructor. It applies independently to each tuple, array, struct, record, or struct-with-rest node and does not make Union candidates concurrent.

- `SchemaAST.ConcurrencyAnnotationId` -> `SchemaAST.ParseOptions`: The annotation ID was removed. Pass concurrency in runtime ParseOptions; product parsing follows Effect.forEach concurrency semantics while Union candidates remain sequential.

- `SchemaAST.Declaration` -> `SchemaAST.Declaration`: The name remains, but its constructor and fields changed in the v4 Base/check/context/encoding model.

- `SchemaAST.DecodingFallbackAnnotation` -> `Schema.catchDecoding`: Fallbacks are now encoding middleware added with Schema.catchDecoding.

- `SchemaAST.DecodingFallbackAnnotationId` -> `Schema.catchDecoding`: The symbol annotation was removed; attach decoding recovery with Schema.catchDecoding.

- `SchemaAST.DefaultAnnotation` -> `Schema.Annotations.Documentation["default"]`: Defaults are string-keyed schema annotations in v4.

- `SchemaAST.DefaultAnnotationId` -> `Schema.Annotations.Documentation["default"]`: Symbol annotation IDs were removed; use the default key.

- `SchemaAST.DescriptionAnnotation` -> `Schema.Annotations.Augment["description"]`: Descriptions are string-keyed schema annotations in v4.

- `SchemaAST.DescriptionAnnotationId` -> `Schema.Annotations.Augment["description"]`: Symbol annotation IDs were removed; use the description key.

- `SchemaAST.DocumentationAnnotation` -> `Schema.Annotations.Augment["documentation"]`: Documentation is a string-keyed schema annotation in v4.

- `SchemaAST.DocumentationAnnotationId` -> `Schema.Annotations.Augment["documentation"]`: Symbol annotation IDs were removed; use the documentation key.

- `SchemaAST.Enums` -> `SchemaAST.Enum`: Renamed to Enum in the v4 AST model. Numeric enum members must be finite; NaN and positive or negative Infinity throw at construction.

- `SchemaAST.EquivalenceAnnotation` -> `Schema.Annotations.ToEquivalence.Declaration`: Equivalence derivation annotations now use the toEquivalence key in Schema.Annotations.

- `SchemaAST.EquivalenceAnnotationId` -> `Schema.overrideToEquivalence`: The symbol annotation was removed; attach custom equivalence derivation with Schema.overrideToEquivalence.

- `SchemaAST.ExamplesAnnotation` -> `Schema.Annotations.Documentation["examples"]`: Examples are string-keyed schema annotations in v4.

- `SchemaAST.ExamplesAnnotationId` -> `Schema.Annotations.Documentation["examples"]`: Symbol annotation IDs were removed; use the examples key.

- `SchemaAST.FinalTransformation` -> `SchemaTransformation.Transformation`: Transformations moved to SchemaTransformation and are stored in SchemaAST.Link values.

- `SchemaAST.IdentifierAnnotation` -> `Schema.Annotations.Bottom["identifier"]`: Identifiers are string-keyed schema annotations in v4.

- `SchemaAST.IdentifierAnnotationId` -> `Schema.Annotations.Bottom["identifier"]`: Symbol annotation IDs were removed; use the identifier key.

- `SchemaAST.IndexSignature` -> `SchemaAST.IndexSignature`: The name remains, but its constructor and fields changed in the v4 Base/check/context/encoding model.

- `SchemaAST.JSONIdentifierAnnotationId` -> `Schema.Annotations.Bottom["identifier"]`: The separate JSON identifier symbol was removed; use identifier.

- `SchemaAST.JSONSchemaAnnotation` -> `JsonSchema.JsonSchema`: JSON Schema values use the v4 JsonSchema model; generation hooks use Schema representation annotations.

- `SchemaAST.JSONSchemaAnnotationId` -> `Schema.Annotations.Filter["toJsonSchema"]`: The symbol annotation was replaced by the toJsonSchema key on check annotations.

- `SchemaAST.Literal` -> `SchemaAST.Literal`: The name remains, but its constructor and fields changed in the v4 Base/check/context/encoding model.

- `SchemaAST.LiteralValue` -> `SchemaAST.LiteralValue`: The name remains, but its constructor and fields changed in the v4 Base/check/context/encoding model.

- `SchemaAST.Match` -> `none`: The Match compiler table was removed; traverse the discriminated SchemaAST.AST union directly.

- `SchemaAST.Members` -> `ReadonlyArray<SchemaAST.AST>`: Union members are ordinary readonly arrays in v4.

- `SchemaAST.MessageAnnotation` -> `Schema.Annotations.Bottom["message"]`: Messages are string-keyed annotations and no longer receive the old ParseIssue callback shape.

- `SchemaAST.MessageAnnotationId` -> `Schema.Annotations.Bottom["message"]`: Symbol annotation IDs were removed; use the message key.

- `SchemaAST.MissingMessageAnnotation` -> `Schema.Annotations.Key["messageMissingKey"]`: Missing-key messages use the messageMissingKey key.

- `SchemaAST.MissingMessageAnnotationId` -> `Schema.Annotations.Key["messageMissingKey"]`: Symbol annotation IDs were removed; use messageMissingKey.

- `SchemaAST.NeverKeyword` -> `SchemaAST.Never`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.NumberKeyword` -> `SchemaAST.Number`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.ObjectKeyword` -> `SchemaAST.ObjectKeyword`: The name remains, but its constructor and fields changed in the v4 Base/check/context/encoding model.

- `SchemaAST.OptionalType` -> `SchemaAST.Context`: Element and property optionality moved into per-node Context.

- `SchemaAST.Parameter` -> `SchemaAST.AST`: The dedicated index-parameter union was removed; v4 validates supported key AST variants when building an IndexSignature.

- `SchemaAST.ParseIssueTitleAnnotation` -> `none`: Issue-title callbacks were removed; use message or expected annotations and SchemaIssue formatters.

- `SchemaAST.ParseIssueTitleAnnotationId` -> `none`: The symbol annotation was removed; use message or expected annotations.

- `SchemaAST.ParseJsonSchemaId` -> `Schema.fromJsonString(Schema.Unknown)`: Use the built-in JSON string codec instead of checking the old schema ID.

- `SchemaAST.ParseOptions` -> `SchemaAST.ParseOptions`: Pass parsing options at runtime. onExcessProperty supports ignore or error, not preserve; model extra values with an explicit Record or StructWithRest. propertyOrder was removed. concurrency follows Effect.forEach semantics for tuple, array, struct, record, and struct-with-rest children, applies independently at each nested product, and does not make Union candidates concurrent. Output key order is unspecified, including in values passed to checks. Handle required presentation or serialization order explicitly outside the parser.

- `SchemaAST.ParseOptionsAnnotationId` -> `none`: Parse options are no longer schema annotations. Pass options when creating or calling a decoder or encoder; there is no annotation-based override for nested schemas.

- `SchemaAST.PrettyAnnotationId` -> `Schema.overrideToFormatter`: The symbol annotation was removed; attach custom formatters with Schema.overrideToFormatter.

- `SchemaAST.PropertySignature` -> `SchemaAST.PropertySignature`: The name remains, but its constructor and fields changed in the v4 Base/check/context/encoding model.

- `SchemaAST.PropertySignatureTransformation` -> `Schema.encodeKeys`: Property-key transformations are now encoding links, normally built with Schema.encodeKeys.

- `SchemaAST.Refinement` -> `SchemaAST.Check`: Refinements became Filter or FilterGroup checks attached to an AST node.

- `SchemaAST.SchemaIdAnnotation` -> `Schema.Annotations.Bottom["identifier"]`: Schema IDs were consolidated into identifier annotations.

- `SchemaAST.SchemaIdAnnotationId` -> `Schema.Annotations.Bottom["identifier"]`: Symbol annotation IDs were removed; use identifier.

- `SchemaAST.StringKeyword` -> `SchemaAST.String`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.SurrogateAnnotation` -> `SchemaRepresentation.RepresentationAnnotation`: Surrogate AST metadata was replaced by schema representation annotations and declaration codec hooks.

- `SchemaAST.SurrogateAnnotationId` -> `Schema.Annotations.Declaration["representation"]`: The symbol annotation was replaced by the representation key.

- `SchemaAST.Suspend` -> `SchemaAST.Suspend`: The name remains, but its constructor and fields changed in the v4 Base/check/context/encoding model.

- `SchemaAST.SymbolKeyword` -> `SchemaAST.Symbol`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.TemplateLiteral` -> `SchemaAST.TemplateLiteral`: The name remains, but its constructor and fields changed in the v4 Base/check/context/encoding model. Parts must not contain encodings, including inside unions or nested templates; use Schema.TemplateLiteralParser for transformed parts.

- `SchemaAST.TemplateLiteralSpan` -> `SchemaAST.TemplateLiteral`: Template literal parts are represented directly as AST values in v4.

- `SchemaAST.TitleAnnotation` -> `Schema.Annotations.Augment["title"]`: Titles are string-keyed schema annotations in v4.

- `SchemaAST.TitleAnnotationId` -> `Schema.Annotations.Augment["title"]`: Symbol annotation IDs were removed; use title.

- `SchemaAST.Transformation` -> `SchemaAST.Link`: Transformations are links in the Base.encoding chain in v4.

- `SchemaAST.TransformationKind` -> `SchemaTransformation.Transformation`: Transformation implementations moved to SchemaTransformation and are stored on SchemaAST.Link.

- `SchemaAST.TupleType` -> `SchemaAST.Arrays`: Use Arrays(isMutable, elements, rest, annotations?, checks?, encoding?, context?, encodingChecks?).

- `SchemaAST.Type` -> `SchemaAST.AST`: The tuple-element Type wrapper was removed; optionality and mutability moved to Context.

- `SchemaAST.TypeConstructorAnnotation` -> `Schema.Annotations.Declaration["toCodec"]`: Type-constructor behavior moved to declaration codec annotations.

- `SchemaAST.TypeConstructorAnnotationId` -> `Schema.Annotations.Declaration["toCodec"]`: Symbol annotation IDs were removed; use declaration codec annotation keys.

- `SchemaAST.TypeLiteral` -> `SchemaAST.Objects`: Use Objects(propertySignatures, indexSignatures, annotations?, checks?, encoding?, context?, encodingChecks?). Output key order is unspecified.

- `SchemaAST.TypeLiteralTransformation` -> `SchemaAST.Encoding`: Object transformations are encoding links; use Schema.encodeKeys for key mappings.

- `SchemaAST.UndefinedKeyword` -> `SchemaAST.Undefined`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.Union` -> `SchemaAST.Union`: Pass member ASTs and an optional options object: new SchemaAST.Union(types, { mode: 'oneOf' }). Read options?.mode ?? 'anyOf' instead of a direct mode field.

- `SchemaAST.UniqueSymbol` -> `SchemaAST.UniqueSymbol`: The name remains, but its constructor and fields changed in the v4 Base/check/context/encoding model.

- `SchemaAST.UnknownKeyword` -> `SchemaAST.Unknown`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.VoidKeyword` -> `SchemaAST.Void`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.annotations` -> `SchemaAST.annotate`: Use the v4 annotation helper and string-keyed Schema.Annotations.

- `SchemaAST.anyKeyword` -> `SchemaAST.any`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.bigIntKeyword` -> `SchemaAST.bigInt`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.booleanKeyword` -> `SchemaAST.boolean`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.composeTransformation` -> `SchemaAST.Encoding`: V4 transformations are SchemaAST.Link values in an encoding chain; compose by adding links with SchemaAST.decodeTo.

- `SchemaAST.defaultParseOption` -> `SchemaAST.defaultParseOptions`: The default parse options constant was pluralized.

- `SchemaAST.encodedAST` -> `SchemaAST.toEncoded`: The encoded projection helper was renamed.

- `SchemaAST.encodedBoundAST` -> `SchemaAST.toEncoded`: The separate encoded-bound projection was removed; use the encoded projection and v4 encoding links.

- `SchemaAST.getAnnotation` -> `SchemaAST.resolveAt`: Resolve string-keyed annotations with resolveAt, or use resolveIdentifier, resolveTitle, and resolveDescription.

- `SchemaAST.getBatchingAnnotation` -> `none`: Batching annotations were removed. ParseOptions.concurrency controls product child parsing only; configure request batching separately.

- `SchemaAST.getBrandAnnotation` -> `none`: Runtime brand inspection was removed because Schema.brand is type-only and does not store brand metadata in the AST.

- `SchemaAST.getCompiler` -> `none`: The Match-based compiler was removed; traverse SchemaAST.AST directly or use the relevant Schema derivation API.

- `SchemaAST.getConcurrencyAnnotation` -> `SchemaAST.ParseOptions`: Concurrency is no longer read from schema annotations. Pass it in runtime ParseOptions when creating or calling a parser. It applies to product children, not Union candidates.

- `SchemaAST.getDecodingFallbackAnnotation` -> `none`: Fallbacks are encoding middleware in v4, not readable annotations; attach them with Schema.catchDecoding.

- `SchemaAST.getDefaultAnnotation` -> `SchemaAST.resolveAt("default")`: Resolve the string-keyed default annotation.

- `SchemaAST.getDescriptionAnnotation` -> `SchemaAST.resolveDescription`: Use the dedicated resolved-description helper.

- `SchemaAST.getDocumentationAnnotation` -> `SchemaAST.resolveAt("documentation")`: Resolve the string-keyed documentation annotation.

- `SchemaAST.getExamplesAnnotation` -> `SchemaAST.resolveAt("examples")`: Resolve the string-keyed examples annotation.

- `SchemaAST.getIdentifierAnnotation` -> `SchemaAST.resolveIdentifier`: Use the dedicated resolved-identifier helper.

- `SchemaAST.getJSONIdentifier` -> `SchemaAST.resolveIdentifier`: JSON Schema references now use the normal resolved identifier.

- `SchemaAST.getJSONIdentifierAnnotation` -> `SchemaAST.resolveIdentifier`: The separate JSON identifier annotation was removed; use identifier.

- `SchemaAST.getJSONSchemaAnnotation` -> `SchemaAST.resolveAt("toJsonSchema")`: JSON Schema generation hooks use the string-keyed toJsonSchema annotation on checks.

- `SchemaAST.getMessageAnnotation` -> `SchemaAST.resolveAt("message")`: Resolve the string-keyed message annotation.

- `SchemaAST.getMissingMessageAnnotation` -> `SchemaAST.resolveAt("messageMissingKey")`: Missing-key messages use the messageMissingKey key.

- `SchemaAST.getParseIssueTitleAnnotation` -> `none`: Issue-title callbacks were removed; use message or expected annotations and SchemaIssue formatters.

- `SchemaAST.getParseOptionsAnnotation` -> `none`: Parse options are no longer schema annotations. Pass options when creating or calling a decoder or encoder; there is no annotation-based override for nested schemas.

- `SchemaAST.getPropertySignatures` -> `SchemaAST.Objects.propertySignatures`: Narrow to Objects and read propertySignatures directly.

- `SchemaAST.getSchemaIdAnnotation` -> `SchemaAST.resolveIdentifier`: Schema IDs were consolidated into the identifier annotation.

- `SchemaAST.getSurrogateAnnotation` -> `SchemaAST.resolveAt("representation")`: Surrogate AST annotations were replaced by representation annotations and declaration codec hooks.

- `SchemaAST.getTemplateLiteralCapturingRegExp` -> `none`: The low-level RegExp compiler was removed; use Schema.TemplateLiteral and schema parsing instead.

- `SchemaAST.getTemplateLiteralRegExp` -> `none`: The low-level RegExp compiler was removed; use Schema.TemplateLiteral and schema parsing instead.

- `SchemaAST.getTitleAnnotation` -> `SchemaAST.resolveTitle`: Use the dedicated resolved-title helper.

- `SchemaAST.getTypeConstructorAnnotation` -> `SchemaAST.resolveAt("toCodec")`: Type-constructor behavior moved to declaration codec annotations.

- `SchemaAST.isAnyKeyword` -> `SchemaAST.isAny`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isBigIntKeyword` -> `SchemaAST.isBigInt`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isBooleanKeyword` -> `SchemaAST.isBoolean`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isComposeTransformation` -> `none`: Compose transformation markers were replaced by explicit SchemaAST.Link encoding chains.

- `SchemaAST.isEnums` -> `SchemaAST.isEnum`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isFinalTransformation` -> `SchemaTransformation.Transformation`: Use SchemaTransformation guards or the transformation object stored on a SchemaAST.Link.

- `SchemaAST.isNeverKeyword` -> `SchemaAST.isNever`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isNumberKeyword` -> `SchemaAST.isNumber`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isParameter` -> `SchemaAST.isString`: The Parameter union was removed; inspect the v4 key AST variants directly.

- `SchemaAST.isRefinement` -> `SchemaAST.Check`: Refinement AST nodes became checks attached to Base.checks.

- `SchemaAST.isStringKeyword` -> `SchemaAST.isString`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isSymbolKeyword` -> `SchemaAST.isSymbol`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isTransformation` -> `SchemaAST.Encoding`: Transformation AST nodes became encoding links attached to Base.encoding.

- `SchemaAST.isTupleType` -> `SchemaAST.isArrays`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isTypeLiteral` -> `SchemaAST.isObjects`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isTypeLiteralTransformation` -> `Schema.encodeKeys`: Property-key transformations are represented by encoding links and normally built with Schema.encodeKeys.

- `SchemaAST.isUndefinedKeyword` -> `SchemaAST.isUndefined`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isUnknownKeyword` -> `SchemaAST.isUnknown`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.isVoidKeyword` -> `SchemaAST.isVoid`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.keyof` -> `none`: Low-level SchemaAST.keyof was removed; model the desired key literals explicitly.

- `SchemaAST.mutable` -> `Schema.mutable`: Apply mutability at the Schema level; AST property mutability is represented by Context.

- `SchemaAST.neverKeyword` -> `SchemaAST.never`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.null` -> `SchemaAST.null`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.numberKeyword` -> `SchemaAST.number`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.omit` -> `Schema.mapFields + Struct.omit`: Object projection moved to schema field transforms.

- `SchemaAST.partial` -> `Schema.mapFields + Struct.map(Schema.optional)`: Partial object transforms moved to schema field transforms.

- `SchemaAST.pick` -> `none`: The low-level AST picker was removed. Keep field selection at the Schema.Struct level with mapFields and Struct.pick, or discriminate and rebuild custom AST nodes explicitly.

- `SchemaAST.required` -> `Schema.mapFields + Struct.map(Schema.requiredKey)`: Required object transforms moved to schema field transforms.

- `SchemaAST.stringKeyword` -> `SchemaAST.string`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.symbolKeyword` -> `SchemaAST.symbol`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.typeAST` -> `SchemaAST.toType`: The type-side projection helper was renamed.

- `SchemaAST.undefinedKeyword` -> `SchemaAST.undefined`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.unknownKeyword` -> `SchemaAST.unknown`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.

- `SchemaAST.voidKeyword` -> `SchemaAST.void`: The v4 SchemaAST redesign renamed this primitive, collection, or guard while preserving its role.
