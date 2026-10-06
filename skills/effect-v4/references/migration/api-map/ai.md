# v3 → v4 API map: @effect/ai (12 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/ai/AiError`

- `AiError.AiError` -> `AiError.AiError`: Moved to effect/ai/AiError and redesigned from a union of separately tagged errors into one AiError wrapper with a semantic reason. Construct it with AiError.make({ module, method, reason }) and match error.reason rather than the old top-level tags.

- `AiError.HttpRequestDetails` -> `AiError.HttpRequestDetails`: Retained in effect/ai/AiError and also re-exported as Response.HttpRequestDetails. Hash is an optional string instead of Option, headers may contain Redacted strings, and method includes TRACE.

- `AiError.HttpRequestError` -> `AiError.make + AiError.NetworkError`: Replace the old top-level request error with an AiError whose reason is NetworkError. NetworkError.fromRequestError converts a v4 HttpClientError.RequestError.

- `AiError.HttpResponseError` -> `AiError.make + AiError.reasonFromHttpStatus / AiError.InvalidOutputError`: There is no single v4 response-error class. Wrap a semantic reason with AiError.make: use reasonFromHttpStatus for status failures and InvalidOutputError for decode or empty-body failures.

- `AiError.MalformedInput` -> `AiError.make + AiError.InvalidUserInputError`: Replace the old top-level input error with an AiError whose reason is InvalidUserInputError. Use InvalidRequestError when the provider request parameters themselves are malformed.

- `AiError.MalformedOutput` -> `AiError.make + AiError.InvalidOutputError`: Replace the old top-level output error with an AiError whose reason is InvalidOutputError. The old fromParseError helper becomes InvalidOutputError.fromSchemaError.

- `AiError.TypeId` -> `AiError.isAiError`: The AiError brand is private in v4. Use isAiError for runtime narrowing, or isAiErrorReason for a reason, instead of inspecting or constructing the type id.

- `AiError.UnknownError` -> `AiError.make + AiError.UnknownError`: UnknownError is now a semantic reason rather than a top-level error. Put module and method on AiError.make and inspect reason.\_tag when handling the outer AiError.

## `@effect/ai/Chat`

- `Chat.Chat` -> `Chat.Chat`: Use the Chat interface for implementations and the Context.Service value as the service key; it is no longer a class. Chat.empty and Chat.fromPrompt construct branded chat sessions.

- `Chat.Service` -> `Chat.Chat`: Service was renamed to Chat. Implementations require [Chat.TypeId]: Chat.TypeId; use Chat.empty or Chat.fromPrompt to create a session.

## `@effect/ai/EmbeddingModel`

- `EmbeddingModel.EmbeddingModel` -> `EmbeddingModel.EmbeddingModel`: Use the EmbeddingModel interface for implementations and the Context.Service value as the service key; it is no longer a class. Prefer EmbeddingModel.make to construct the branded implementation.

- `EmbeddingModel.Result`: TODO: needs guidance

- `EmbeddingModel.Service` -> `EmbeddingModel.EmbeddingModel`: Service was renamed to EmbeddingModel. Implementations require [EmbeddingModel.TypeId]: EmbeddingModel.TypeId; EmbeddingModel.make supplies the brand.

- `EmbeddingModel.makeDataLoader` -> `EmbeddingModel.make + RequestResolver.setDelay + RequestResolver.batchN`: The dedicated data-loader constructor was removed. EmbeddingModel.make batches concurrent embed requests through its resolver; compose the exposed resolver with setDelay and optional batchN for the old window and maximum-batch behavior.

## `@effect/ai/IdGenerator`

- `IdGenerator.make` -> `IdGenerator.make`: Moved to effect/ai/IdGenerator with the same configurable alphabet, prefix, separator, and size behavior. Invalid configuration now fails with Cause.IllegalArgumentError.

## `@effect/ai/LanguageModel`

- `LanguageModel.ConstructorParams` -> `none`: V4 inlines this provider-adapter shape in LanguageModel.make. Pass generateText and streamText directly to make, with optional codecTransformer, instead of naming a constructor-parameter type.

- `LanguageModel.ExtractContext` -> `LanguageModel.ExtractServices`: Renamed in effect/ai/LanguageModel. ExtractServices infers toolkit handler, result-decoding, and effectful-toolkit service requirements.

- `LanguageModel.LanguageModel` -> `LanguageModel.LanguageModel`: Use the LanguageModel interface for implementations and the Context.Service value as the service key; it is no longer a class. Prefer LanguageModel.make to construct the branded implementation.

- `LanguageModel.Service` -> `LanguageModel.LanguageModel`: Service was renamed to LanguageModel. Implementations require [LanguageModel.TypeId]: LanguageModel.TypeId; LanguageModel.make supplies the brand.

## `@effect/ai/McpSchema`

- `McpSchema.ContentBlock` -> `McpSchema.ContentBlock`: Moved to effect/ai/McpSchema. It remains the MCP content-block union, but v4 exports it as a const schema rather than a Schema.Union subclass. Binary image, audio, and blob data still use Uint8Array values with base64 wire encoding.

- `McpSchema.ElicitResult` -> `McpSchema.ElicitResult`: Moved to effect/ai/McpSchema. It remains discriminated by action, but accepted content is now optional and, when present, is a record of strings, finite numbers, booleans, or string arrays; declined and canceled responses still omit content.

- `McpSchema.McpError` -> `McpSchema.McpError`: Moved, but changed from a constructable base class to a union schema of standard tagged protocol errors plus McpErrorBase. Use McpErrorBase to construct a generic MCP error.

- `McpSchema.ParamAnnotation` -> `McpSchema.isParam / Param.name`: The public symbol annotation was removed. Detect parameter wrappers with McpSchema.isParam and read the narrowed Param.name instead of inspecting AST annotations.

- `McpSchema.param` -> `McpSchema.param`: Moved to effect/ai/McpSchema. V4 wraps the schema and exposes Param.name and Param.schema instead of attaching a public symbol annotation.

## `@effect/ai/McpServer`

- `McpServer.McpServer` -> `McpServer.McpServer`: The service is now a Context.Service. initializedClients and notificationsMailbox are no longer exposed. Registration callbacks use McpSchema.McpRequestContext; McpServerClient is only supplied for initialized stateful requests. Prefer registerToolkit, registerResource, and registerPrompt over implementing the registry shape directly.

- `McpServer.layer` -> `McpServer.layer`: Moved to effect/ai/McpServer. Pass a non-empty protocols array of adapters, such as [McpProtocol.v2025\_06\_18], imported with McpProtocol from effect/ai; it still runs over a caller-provided RpcServer.Protocol.

- `McpServer.layerHttp` -> `McpServer.layerHttp`: Moved to effect/ai/McpServer and the unified HttpRouter. Pass a non-empty protocols array of adapters, such as [McpProtocol.v2025\_06\_18], imported with McpProtocol from effect/ai.

- `McpServer.layerHttpRouter` -> `McpServer.layerHttp`: Renamed and consolidated. V4 layerHttp registers the Streamable HTTP endpoint in the unified HttpRouter; pass a non-empty protocols array of adapters, such as [McpProtocol.v2025\_06\_18], imported with McpProtocol from effect/ai.

- `McpServer.layerStdio` -> `McpServer.layerStdio`: Moved to effect/ai/McpServer. Pass a non-empty protocols array of adapters, such as [McpProtocol.v2025\_06\_18], imported with McpProtocol from effect/ai.

- `McpServer.prompt` -> `McpServer.prompt`: Moved to effect/ai/McpServer. The registration layer excludes McpSchema.McpRequestContext from prompt decoding and handler requirements instead of McpServerClient.

- `McpServer.registerPrompt` -> `McpServer.registerPrompt`: Moved to effect/ai/McpServer. Prompt decoding and handler requirements now exclude McpSchema.McpRequestContext instead of McpServerClient; use the request context for protocol-neutral client metadata.

- `McpServer.registerResource` -> `McpServer.registerResource`: Moved to effect/ai/McpServer. Resource and completion handler requirements now exclude McpSchema.McpRequestContext instead of McpServerClient; use the request context for protocol-neutral client metadata.

- `McpServer.registerToolkit` -> `McpServer.registerToolkit`: Moved to effect/ai/McpServer. Handler requirements now exclude McpSchema.McpRequestContext instead of McpServerClient. Strict tools reject excess input properties; raw JSON Schema dynamic tools cannot use strict mode. Declared handler failures produce isError results, while parameter validation fails with InvalidParams.

- `McpServer.resource` -> `McpServer.resource`: Moved to effect/ai/McpServer. The registration layer excludes McpSchema.McpRequestContext from resource and completion handler requirements instead of McpServerClient.

- `McpServer.run` -> `McpServer.run`: Moved to effect/ai/McpServer. Pass a non-empty protocols array of adapters, such as [McpProtocol.v2025\_06\_18], imported with McpProtocol from effect/ai; it remains the Effect-level runner over RpcServer.Protocol.

- `McpServer.toolkit` -> `McpServer.toolkit`: Moved to effect/ai/McpServer. The registration layer supplies McpSchema.McpRequestContext to handlers instead of excluding McpServerClient from requirements. Strict tools reject excess properties and require an Effect Schema rather than raw dynamic JSON Schema.

## `@effect/ai/Model`

- `Model.TypeId` -> `none`: The Model brand still exists internally, but its TypeId is not exported and v4 has no public isModel guard. Use Model values created by Model.make rather than inspecting or constructing the brand.

## `@effect/ai/Prompt`

- `Prompt.FilePart` -> `Prompt.FilePart`: Moved to effect/ai/Prompt with the same file-part model and schema; update the module import.

- `Prompt.FromJson` -> `Schema.fromJsonString(Prompt.Prompt)`: The module-specific JSON schema was removed. Compose the general v4 JSON-string codec with the public Prompt codec.

- `Prompt.MessageContentFromString` -> `Prompt.ContentFromString`: Renamed in effect/ai/Prompt. It still decodes a string to a non-empty array containing one TextPart and encodes the first part's text.

- `Prompt.MessageTypeId` -> `Prompt.isMessage`: The message type id is private in v4. Use the public isMessage guard for runtime refinement instead of importing or inspecting the marker.

- `Prompt.Part` -> `Prompt.Part`: Moved to effect/ai/Prompt. The union now also includes tool-approval request and response parts.

- `Prompt.PartEncoded` -> `Prompt.PartEncoded`: Moved to effect/ai/Prompt. The encoded union now also includes tool-approval request and response parts.

- `Prompt.PartTypeId` -> `Prompt.isPart`: The part type id is private in v4. Use the public isPart guard for runtime refinement instead of importing or inspecting the marker.

- `Prompt.PromptFromSelf` -> `Prompt.Prompt`: The standalone declared from-self schema was removed. Use the public Prompt codec for prompt validation and encoding, or Prompt.isPrompt when only runtime refinement is needed.

- `Prompt.ReasoningPart` -> `Prompt.ReasoningPart`: Moved to effect/ai/Prompt with the same reasoning-part model and schema; update the module import.

- `Prompt.TextPart` -> `Prompt.TextPart`: Moved to effect/ai/Prompt with the same text-part model and schema; update the module import.

- `Prompt.ToolCallPart` -> `Prompt.ToolCallPart`: Moved to effect/ai/Prompt with the same tool-call model and schema; update the module import.

- `Prompt.ToolResultPartEncoded` -> `Prompt.ToolResultPartEncoded`: Moved to effect/ai/Prompt, but providerExecuted was removed from the encoded prompt tool-result shape.

- `Prompt.TypeId` -> `Prompt.isPrompt`: The Prompt type id is private in v4 and its internal literal changed. Use the public isPrompt guard instead of importing or inspecting the marker.

- `Prompt.makePart` -> `Prompt.makePart`: Moved to effect/ai/Prompt. The generic constructor also supports the new tool-approval request and response part variants.

- `Prompt.merge` -> `Prompt.concat`: Renamed in v4. concat preserves the old dual API and concatenates the messages from a Prompt with additional raw input.

## `@effect/ai/Response`

- `Response.Part` -> `Response.Part`: Moved to effect/ai/Response. The non-streaming union now also includes ToolApprovalRequestPart.

- `Response.PartTypeId` -> `Response.isPart`: The public PartTypeId was removed and the marker is internal in v4. Use Response.isPart for runtime refinement.

- `Response.ToolCallPartEncoded` -> `Response.ToolCallPartEncoded`: Moved to effect/ai/Response; providerName was removed while providerExecuted remains optional when encoded.

- `Response.ToolResultPart` -> `Response.ToolResultPart`: The schema selects success or failure using isFailure rather than trying both result schemas. It returns Schema.Codec; supply decoding services when decoding and encoding services when encoding.

- `Response.ToolResultPartEncoded` -> `Response.ToolResultPartEncoded`: Moved to effect/ai/Response; providerName was removed and optional preliminary was added to the encoded shape.

- `Response.documentSourcePart` -> `Response.makePart("source", { ...params, sourceType: "document" })`: The lowercase convenience constructor was removed. The DocumentSourcePart model remains, and the generic constructor now requires the document source discriminator.

- `Response.errorPart` -> `Response.makePart("error", params)`: The lowercase convenience constructor was removed; construct the retained error part through Response.makePart.

- `Response.finishPart` -> `Response.makePart("finish", params)`: The lowercase convenience constructor was removed. V4 Usage has nested inputTokens and outputTokens objects, and FinishPart adds optional HTTP response details.

- `Response.reasoningDeltaPart` -> `Response.makePart("reasoning-delta", params)`: The lowercase convenience constructor was removed; construct the retained ReasoningDeltaPart through Response.makePart.

- `Response.reasoningEndPart` -> `Response.makePart("reasoning-end", params)`: The lowercase convenience constructor was removed; construct the retained ReasoningEndPart through Response.makePart.

- `Response.reasoningStartPart` -> `Response.makePart("reasoning-start", params)`: The lowercase convenience constructor was removed; construct the retained ReasoningStartPart through Response.makePart.

- `Response.responseMetadataPart` -> `Response.makePart("response-metadata", params)`: The lowercase convenience constructor was removed. V4 id, modelId, and timestamp are optional raw values rather than Option values, and optional HTTP request details were added.

- `Response.textDeltaPart` -> `Response.makePart("text-delta", params)`: The lowercase convenience constructor was removed; construct the retained TextDeltaPart through Response.makePart.

- `Response.textEndPart` -> `Response.makePart("text-end", params)`: The lowercase convenience constructor was removed; construct the retained TextEndPart through Response.makePart.

- `Response.textStartPart` -> `Response.makePart("text-start", params)`: The lowercase convenience constructor was removed; construct the retained TextStartPart through Response.makePart.

- `Response.toolParamsDeltaPart` -> `Response.makePart("tool-params-delta", params)`: The lowercase convenience constructor was removed; construct the retained ToolParamsDeltaPart through Response.makePart.

- `Response.toolParamsEndPart` -> `Response.makePart("tool-params-end", params)`: The lowercase convenience constructor was removed; construct the retained ToolParamsEndPart through Response.makePart.

- `Response.toolParamsStartPart` -> `Response.makePart("tool-params-start", params)`: The lowercase convenience constructor was removed; providerName was also removed from ToolParamsStartPart in v4.

- `Response.toolResultPart` -> `Response.toolResultPart`: Moved to effect/ai/Response; providerName was removed and decoded tool results now require preliminary, normally false.

- `Response.urlSourcePart` -> `Response.makePart("source", { ...params, sourceType: "url" })`: The lowercase convenience constructor was removed. The UrlSourcePart model remains, and the generic constructor now requires the URL source discriminator.

## `@effect/ai/Tool`

- `Tool.AnyParametersSchema` -> `Schema.Constraint`: The AI-specific alias was removed. V4 Tool parameter schemas use the general Schema.Constraint type and are no longer restricted to the old struct-or-EmptyParams union.

- `Tool.AnyTaggedRequestSchema` -> `none`: The TaggedRequest-specific Tool adapter contract was removed. Model the operation directly with Tool.make and ordinary v4 Schema.Constraint values.

- `Tool.Destructive` -> `Tool.Destructive`: Moved to effect/ai/Tool. It is now a Context.Reference\<boolean\> value rather than a Reference subclass; its default remains true.

- `Tool.Failure` -> `Tool.Failure`: Moved to effect/ai/Tool and remains the utility type that extracts a tool's decoded failure type.

- `Tool.FromTaggedRequest` -> `Tool.Tool`: The dedicated derived alias was removed. Construct with Tool.make and let Tool.Tool infer the name, parameter, success, and failure schemas.

- `Tool.Idempotent` -> `Tool.Idempotent`: Moved to effect/ai/Tool. It is now a Context.Reference\<boolean\> value rather than a Reference subclass; its default remains false.

- `Tool.OpenWorld` -> `Tool.OpenWorld`: Moved to effect/ai/Tool. It is now a Context.Reference\<boolean\> value rather than a Reference subclass; its default remains true.

- `Tool.ProviderDefinedTypeId` -> `Tool.ProviderDefinedTypeId`: Moved to effect/ai/Tool and remains public. Its literal changed, so use the export rather than retaining the old hard-coded string.

- `Tool.Readonly` -> `Tool.Readonly`: Moved to effect/ai/Tool. It is now a Context.Reference\<boolean\> value rather than a Reference subclass; its default remains false.

- `Tool.Requirements` -> `Tool.HandlerServices`: Renamed and refined. HandlerServices combines parameter-decoding, result-encoding, and request-level dependencies required by a tool handler.

- `Tool.Result` -> `Tool.Result`: The result includes success, declared failure, and execution-denied or execution-interrupted values in both failure modes. Return mode also includes AiError; handle these variants when narrowing results.

- `Tool.ResultEncoded` -> `Tool.ResultEncoded`: The encoded result includes execution-denied and execution-interrupted variants in both failure modes, plus encoded AiError in return mode.

- `Tool.Success` -> `Tool.Success`: Moved to effect/ai/Tool and remains the utility type that extracts a tool's decoded success type.

- `Tool.Tool.ProviderDefinedProto` -> `Tool.ProviderDefined`: This implementation-brand interface is no longer public. Use Tool.ProviderDefined for the model type and Tool.isProviderDefined for runtime narrowing.

- `Tool.Tool.Variance` -> `Tool.Tool / Tool.Any`: This implementation variance interface is no longer public; its requirement marker is inline in Tool.Tool. Constrain generic code with Tool.Tool or Tool.Any.

- `Tool.Tool.VarianceStruct` -> `Tool.Tool / Tool.Any`: This implementation variance payload is no longer public; the requirements marker is inline in Tool.Tool and should not be named independently.

- `Tool.TypeId` -> `Tool.TypeId`: Moved to effect/ai/Tool and remains public. Its literal changed, so use the export rather than retaining the old hard-coded string.

- `Tool.fromTaggedRequest` -> `Tool.make`: The adapter was removed. Rebuild the tool explicitly with Tool.make(name, { parameters, success, failure }); Toolkit.make no longer converts schema values automatically.

- `Tool.getDescriptionFromSchemaAst` -> `SchemaAST.resolveDescription`: Moved out of Tool to the general v4 AST annotation resolver. For a Tool value, prefer Tool.getDescription.

- `Tool.getJsonSchemaFromSchemaAst` -> `Tool.getJsonSchemaFromSchema`: Renamed to accept a Schema.Constraint instead of a raw AST and now emits the v4 JSON Schema model. Wrap a raw AST with Schema.make first.

## `@effect/ai/Toolkit`

- `Toolkit.HandlersFrom` -> `Toolkit.HandlersFrom`: Moved to effect/ai/Toolkit. V4 handlers receive a HandlerContext argument and may fail with the declared failure, AiError, or AiErrorReason while requiring Tool.HandlerServices.

- `Toolkit.TypeId` -> `Toolkit.Toolkit / Toolkit.Any`: The toolkit nominal id is private in v4. Use Toolkit.Toolkit or Toolkit.Any for typing instead of importing or inspecting the marker.
