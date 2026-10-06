# v3 → v4 API map: @effect/ai-anthropic (6 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/ai-anthropic/AnthropicClient`

- `AnthropicClient.CitationsDelta` -> `Generated.BetaCitationsDelta`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.ContentBlockDeltaEvent` -> `Generated.BetaContentBlockDeltaEvent`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.ContentBlockStartEvent` -> `Generated.BetaContentBlockStartEvent`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.ContentBlockStopEvent` -> `Generated.BetaContentBlockStopEvent`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.ErrorEvent` -> `Generated.BetaErrorResponse`: The client-local stream error schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.InputJsonContentBlockDelta` -> `Generated.BetaInputJsonContentBlockDelta`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.MessageDelta` -> `Generated.BetaMessageDelta`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.MessageDeltaEvent` -> `Generated.BetaMessageDeltaEvent`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.MessageDeltaUsage` -> `typeof Generated.BetaMessageDeltaEvent.Type["usage"]`: The standalone usage schema was inlined into the regenerated v4 message-delta event.

- `AnthropicClient.MessageStartEvent` -> `Generated.BetaMessageStartEvent`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.MessageStopEvent` -> `Generated.BetaMessageStopEvent`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.MessageStreamEvent` -> `AnthropicClient.MessageStreamEvent`: Still exported in v4 as a type union of generated beta stream events; adapt to the revised client stream contract.

- `AnthropicClient.PingEvent` -> `none`: The v4 client consumes ping events internally and filters them from MessageStreamEvent, so no public ping schema is needed.

- `AnthropicClient.ServerToolUsage` -> `Generated.BetaServerToolUsage`: The client-local usage schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.Service` -> `AnthropicClient.Service`: Still exported in v4; adapt to the revised generated client, streamRequest, and message response contracts.

- `AnthropicClient.SignatureContentBlockDelta` -> `Generated.BetaSignatureContentBlockDelta`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.TextContentBlockDelta` -> `Generated.BetaTextContentBlockDelta`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

- `AnthropicClient.ThinkingContentBlockDelta` -> `Generated.BetaThinkingContentBlockDelta`: The client-local stream schema moved to the regenerated v4 Anthropic schema surface; re-check its Type/Encoded shape.

## `@effect/ai-anthropic/AnthropicConfig`

- `AnthropicConfig.AnthropicConfig` -> `AnthropicConfig.AnthropicConfig`: Still exported in v4; update imports and adapt to the revised v4 service and HTTP client types.

- `AnthropicConfig.AnthropicConfig.Service` -> `AnthropicConfig.AnthropicConfig.Service`: Still exported in v4; update imports and adapt to the revised v4 service and HTTP client types.

## `@effect/ai-anthropic/AnthropicLanguageModel`

- `AnthropicLanguageModel.AnthropicReasoningInfo` -> `Prompt.ReasoningPartOptions / Response reasoning metadata`: The standalone reasoning-info union was removed; v4 declares Anthropic thinking and redacted-thinking data directly on Prompt and Response provider metadata.

- `AnthropicLanguageModel.AnthropicTools` -> `AnthropicLanguageModel.AnthropicUserDefinedTool | AnthropicLanguageModel.AnthropicProviderDefinedTool`: The old combined tool union was split into explicit user-defined and provider-defined Anthropic request tool types.

- `AnthropicLanguageModel.Config` -> `AnthropicLanguageModel.Config`: Still exported in v4; update imports and adapt to the revised Messages API request fields.

- `AnthropicLanguageModel.Config.Service` -> `AnthropicLanguageModel.Config.Service`: Still exported in v4; update imports and adapt to the revised Messages API request fields.

- `AnthropicLanguageModel.layerWithTokenizer` -> `AnthropicLanguageModel.layer`: The tokenizer-combining layer was removed; provide the language model and any Tokenizer service separately.

- `AnthropicLanguageModel.modelWithTokenizer` -> `AnthropicLanguageModel.model`: The tokenizer-combining model was removed; use the v4 model descriptor and provide any Tokenizer service separately.

- `AnthropicLanguageModel.prepareTools` -> `none`: Tool conversion became an internal part of the v4 Anthropic language model; use AnthropicTool constructors and pass tools through LanguageModel provider options instead.

## `@effect/ai-anthropic/AnthropicTokenizer`

- `AnthropicTokenizer.layer` -> `Tokenizer.make`: The provider-specific tokenizer module was removed; build and provide an effect/ai/Tokenizer service explicitly, using @anthropic-ai/tokenizer if equivalent Anthropic counting is required.

- `AnthropicTokenizer.make` -> `Tokenizer.make`: The provider-specific tokenizer module was removed; build and provide an effect/ai/Tokenizer service explicitly, using @anthropic-ai/tokenizer if equivalent Anthropic counting is required.

## `@effect/ai-anthropic/AnthropicTool`

- `AnthropicTool.ProviderDefinedTools` -> `AnthropicTool.AnthropicTool`: The provider-defined schema union was replaced by the union of v4 Anthropic provider tool constructor return types.

- `AnthropicTool.getProviderDefinedToolName` -> `Tool.NameMapper`: The Anthropic-specific name lookup was removed; v4 provider tools carry custom and provider names through the shared Tool.NameMapper.

## `@effect/ai-anthropic/Generated`

- `Generated.APIError` -> `Generated.APIError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.AuthenticationError` -> `Generated.AuthenticationError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Base64ImageSource` -> `Generated.Base64ImageSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Base64ImageSourceMediaType` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.Base64PDFSource` -> `Generated.Base64PDFSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BashTool20250124` -> `Generated.BashTool_20250124`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaAPIError` -> `Generated.BetaAPIError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaAuthenticationError` -> `Generated.BetaAuthenticationError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaBase64ImageSource` -> `Generated.BetaBase64ImageSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaBase64ImageSourceMediaType` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaBase64PDFSource` -> `Generated.BetaBase64PDFSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaBashCodeExecutionToolResultErrorCode` -> `Generated.BetaBashCodeExecutionToolResultErrorCode`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaBashTool20241022` -> `Generated.BetaBashTool_20241022`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaBashTool20250124` -> `Generated.BetaBashTool_20250124`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaBillingError` -> `Generated.BetaBillingError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaBodyCreateSkillV1SkillsPost` -> `Generated.BetaBody_create_skill_v1_skills_post`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaBodyCreateSkillVersionV1SkillsSkillIdVersionsPost` -> `Generated.BetaBody_create_skill_version_v1_skills__skill_id__versions_post`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCacheControlEphemeral` -> `Generated.BetaCacheControlEphemeral`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCacheControlEphemeralTtl` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaCacheCreation` -> `Generated.BetaCacheCreation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaClearToolUses20250919` -> `Generated.BetaClearToolUses20250919`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCodeExecutionTool20250522` -> `Generated.BetaCodeExecutionTool_20250522`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCodeExecutionTool20250825` -> `Generated.BetaCodeExecutionTool_20250825`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCodeExecutionToolResultErrorCode` -> `Generated.BetaCodeExecutionToolResultErrorCode`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaComputerUseTool20241022` -> `Generated.BetaComputerUseTool_20241022`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaComputerUseTool20250124` -> `Generated.BetaComputerUseTool_20250124`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaContainer` -> `Generated.BetaContainer`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaContainerParams` -> `Generated.BetaContainerParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaContentBlock` -> `Generated.BetaContentBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaContentBlockSource` -> `Generated.BetaContentBlockSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaContextManagementConfig` -> `Generated.BetaContextManagementConfig`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaContextManagementResponse` -> `Generated.BetaContextManagementResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCountMessageTokensParams` -> `Generated.BetaCountMessageTokensParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCountMessageTokensResponse` -> `Generated.BetaCountMessageTokensResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCreateMessageBatchParams` -> `Generated.BetaCreateMessageBatchParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCreateMessageParams` -> `Generated.BetaCreateMessageParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCreateMessageParamsServiceTier` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaCreateSkillResponse` -> `Generated.BetaCreateSkillResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCreateSkillV1SkillsPostParams` -> `Generated.BetaCreateSkillV1SkillsPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCreateSkillVersionResponse` -> `Generated.BetaCreateSkillVersionResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaCreateSkillVersionV1SkillsSkillIdVersionsPostParams` -> `Generated.BetaCreateSkillVersionV1SkillsSkillIdVersionsPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaDeleteFileV1FilesFileIdDeleteParams` -> `Generated.BetaDeleteFileV1FilesFileIdDeleteParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaDeleteMessageBatchResponse` -> `Generated.BetaDeleteMessageBatchResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaDeleteSkillResponse` -> `Generated.BetaDeleteSkillResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaDeleteSkillV1SkillsSkillIdDeleteParams` -> `Generated.BetaDeleteSkillV1SkillsSkillIdDeleteParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaDeleteSkillVersionResponse` -> `Generated.BetaDeleteSkillVersionResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaDeleteSkillVersionV1SkillsSkillIdVersionsVersionDeleteParams` -> `Generated.BetaDeleteSkillVersionV1SkillsSkillIdVersionsVersionDeleteParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaDownloadFileV1FilesFileIdContentGetParams` -> `Generated.BetaDownloadFileV1FilesFileIdContentGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaErrorResponse` -> `Generated.BetaErrorResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaFileDeleteResponse` -> `Generated.BetaFileDeleteResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaFileDocumentSource` -> `Generated.BetaFileDocumentSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaFileImageSource` -> `Generated.BetaFileImageSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaFileListResponse` -> `Generated.BetaFileListResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaFileMetadataSchema` -> `Generated.BetaFileMetadataSchema`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaGatewayTimeoutError` -> `Generated.BetaGatewayTimeoutError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaGetFileMetadataV1FilesFileIdGetParams` -> `Generated.BetaGetFileMetadataV1FilesFileIdGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaGetSkillResponse` -> `Generated.BetaGetSkillResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaGetSkillV1SkillsSkillIdGetParams` -> `Generated.BetaGetSkillV1SkillsSkillIdGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaGetSkillVersionResponse` -> `Generated.BetaGetSkillVersionResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaGetSkillVersionV1SkillsSkillIdVersionsVersionGetParams` -> `Generated.BetaGetSkillVersionV1SkillsSkillIdVersionsVersionGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaInputContentBlock` -> `Generated.BetaInputContentBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaInputMessage` -> `Generated.BetaInputMessage`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaInputMessageRole` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaInputSchema` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaInputTokensClearAtLeast` -> `Generated.BetaInputTokensClearAtLeast`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaInputTokensTrigger` -> `Generated.BetaInputTokensTrigger`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaInvalidRequestError` -> `Generated.BetaInvalidRequestError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaListFilesV1FilesGetParams` -> `Generated.BetaListFilesV1FilesGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaListResponseMessageBatch` -> `Generated.BetaListResponse_MessageBatch_`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaListResponseModelInfo` -> `Generated.BetaListResponse_ModelInfo_`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaListSkillVersionsResponse` -> `Generated.BetaListSkillVersionsResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaListSkillVersionsV1SkillsSkillIdVersionsGetParams` -> `Generated.BetaListSkillVersionsV1SkillsSkillIdVersionsGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaListSkillsResponse` -> `Generated.BetaListSkillsResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaListSkillsV1SkillsGetParams` -> `Generated.BetaListSkillsV1SkillsGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMemoryTool20250818` -> `Generated.BetaMemoryTool_20250818`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessage` -> `Generated.BetaMessage`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessageBatch` -> `Generated.BetaMessageBatch`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessageBatchIndividualRequestParams` -> `Generated.BetaMessageBatchIndividualRequestParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessageBatchProcessingStatus` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaMessageBatchesCancelParams` -> `Generated.BetaMessageBatchesCancelParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessageBatchesDeleteParams` -> `Generated.BetaMessageBatchesDeleteParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessageBatchesListParams` -> `Generated.BetaMessageBatchesListParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessageBatchesPostParams` -> `Generated.BetaMessageBatchesPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessageBatchesResultsParams` -> `Generated.BetaMessageBatchesResultsParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessageBatchesRetrieveParams` -> `Generated.BetaMessageBatchesRetrieveParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessagesCountTokensPostParams` -> `Generated.BetaMessagesCountTokensPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMessagesPostParams` -> `Generated.BetaMessagesPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaMetadata` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaModelInfo` -> `Generated.BetaModelInfo`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaModelsGetParams` -> `Generated.BetaModelsGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaModelsListParams` -> `Generated.BetaModelsListParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaNotFoundError` -> `Generated.BetaNotFoundError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaOverloadedError` -> `Generated.BetaOverloadedError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaPermissionError` -> `Generated.BetaPermissionError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaPlainTextSource` -> `Generated.BetaPlainTextSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRateLimitError` -> `Generated.BetaRateLimitError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestBashCodeExecutionOutputBlock` -> `Generated.BetaRequestBashCodeExecutionOutputBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestBashCodeExecutionResultBlock` -> `Generated.BetaRequestBashCodeExecutionResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestBashCodeExecutionToolResultBlock` -> `Generated.BetaRequestBashCodeExecutionToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestBashCodeExecutionToolResultError` -> `Generated.BetaRequestBashCodeExecutionToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestCharLocationCitation` -> `Generated.BetaRequestCharLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestCitationsConfig` -> `Generated.BetaRequestCitationsConfig`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestCodeExecutionOutputBlock` -> `Generated.BetaRequestCodeExecutionOutputBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestCodeExecutionResultBlock` -> `Generated.BetaRequestCodeExecutionResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestCodeExecutionToolResultBlock` -> `Generated.BetaRequestCodeExecutionToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestCodeExecutionToolResultError` -> `Generated.BetaRequestCodeExecutionToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestContainerUploadBlock` -> `Generated.BetaRequestContainerUploadBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestContentBlockLocationCitation` -> `Generated.BetaRequestContentBlockLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestCounts` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaRequestDocumentBlock` -> `Generated.BetaRequestDocumentBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestImageBlock` -> `Generated.BetaRequestImageBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestMCPServerToolConfiguration` -> `Generated.BetaRequestMCPServerToolConfiguration`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestMCPServerURLDefinition` -> `Generated.BetaRequestMCPServerURLDefinition`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestMCPToolResultBlock` -> `Generated.BetaRequestMCPToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestMCPToolUseBlock` -> `Generated.BetaRequestMCPToolUseBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestPageLocationCitation` -> `Generated.BetaRequestPageLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestRedactedThinkingBlock` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaRequestSearchResultBlock` -> `Generated.BetaRequestSearchResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestSearchResultLocationCitation` -> `Generated.BetaRequestSearchResultLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestServerToolUseBlock` -> `Generated.BetaRequestServerToolUseBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestServerToolUseBlockName` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaRequestTextBlock` -> `Generated.BetaRequestTextBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestTextEditorCodeExecutionCreateResultBlock` -> `Generated.BetaRequestTextEditorCodeExecutionCreateResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestTextEditorCodeExecutionStrReplaceResultBlock` -> `Generated.BetaRequestTextEditorCodeExecutionStrReplaceResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestTextEditorCodeExecutionToolResultBlock` -> `Generated.BetaRequestTextEditorCodeExecutionToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestTextEditorCodeExecutionToolResultError` -> `Generated.BetaRequestTextEditorCodeExecutionToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestTextEditorCodeExecutionViewResultBlock` -> `Generated.BetaRequestTextEditorCodeExecutionViewResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestTextEditorCodeExecutionViewResultBlockFileType` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaRequestThinkingBlock` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaRequestToolResultBlock` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaRequestToolUseBlock` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaRequestWebFetchResultBlock` -> `Generated.BetaRequestWebFetchResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestWebFetchToolResultBlock` -> `Generated.BetaRequestWebFetchToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestWebFetchToolResultError` -> `Generated.BetaRequestWebFetchToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestWebSearchResultBlock` -> `Generated.BetaRequestWebSearchResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestWebSearchResultLocationCitation` -> `Generated.BetaRequestWebSearchResultLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestWebSearchToolResultBlock` -> `Generated.BetaRequestWebSearchToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaRequestWebSearchToolResultError` -> `Generated.BetaRequestWebSearchToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseBashCodeExecutionOutputBlock` -> `Generated.BetaResponseBashCodeExecutionOutputBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseBashCodeExecutionResultBlock` -> `Generated.BetaResponseBashCodeExecutionResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseBashCodeExecutionToolResultBlock` -> `Generated.BetaResponseBashCodeExecutionToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseBashCodeExecutionToolResultError` -> `Generated.BetaResponseBashCodeExecutionToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseCharLocationCitation` -> `Generated.BetaResponseCharLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseCitationsConfig` -> `Generated.BetaResponseCitationsConfig`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseClearToolUses20250919Edit` -> `Generated.BetaResponseClearToolUses20250919Edit`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseCodeExecutionOutputBlock` -> `Generated.BetaResponseCodeExecutionOutputBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseCodeExecutionResultBlock` -> `Generated.BetaResponseCodeExecutionResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseCodeExecutionToolResultBlock` -> `Generated.BetaResponseCodeExecutionToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseCodeExecutionToolResultError` -> `Generated.BetaResponseCodeExecutionToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseContainerUploadBlock` -> `Generated.BetaResponseContainerUploadBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseContentBlockLocationCitation` -> `Generated.BetaResponseContentBlockLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseContextManagement` -> `Generated.BetaResponseContextManagement`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseDocumentBlock` -> `Generated.BetaResponseDocumentBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseMCPToolResultBlock` -> `Generated.BetaResponseMCPToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseMCPToolUseBlock` -> `Generated.BetaResponseMCPToolUseBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponsePageLocationCitation` -> `Generated.BetaResponsePageLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseRedactedThinkingBlock` -> `Generated.BetaResponseRedactedThinkingBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseSearchResultLocationCitation` -> `Generated.BetaResponseSearchResultLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseServerToolUseBlock` -> `Generated.BetaResponseServerToolUseBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseServerToolUseBlockName` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaResponseTextBlock` -> `Generated.BetaResponseTextBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseTextEditorCodeExecutionCreateResultBlock` -> `Generated.BetaResponseTextEditorCodeExecutionCreateResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseTextEditorCodeExecutionStrReplaceResultBlock` -> `Generated.BetaResponseTextEditorCodeExecutionStrReplaceResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseTextEditorCodeExecutionToolResultBlock` -> `Generated.BetaResponseTextEditorCodeExecutionToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseTextEditorCodeExecutionToolResultError` -> `Generated.BetaResponseTextEditorCodeExecutionToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseTextEditorCodeExecutionViewResultBlock` -> `Generated.BetaResponseTextEditorCodeExecutionViewResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseTextEditorCodeExecutionViewResultBlockFileType` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaResponseThinkingBlock` -> `Generated.BetaResponseThinkingBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseToolUseBlock` -> `Generated.BetaResponseToolUseBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseWebFetchResultBlock` -> `Generated.BetaResponseWebFetchResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseWebFetchToolResultBlock` -> `Generated.BetaResponseWebFetchToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseWebFetchToolResultError` -> `Generated.BetaResponseWebFetchToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseWebSearchResultBlock` -> `Generated.BetaResponseWebSearchResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseWebSearchResultLocationCitation` -> `Generated.BetaResponseWebSearchResultLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseWebSearchToolResultBlock` -> `Generated.BetaResponseWebSearchToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaResponseWebSearchToolResultError` -> `Generated.BetaResponseWebSearchToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaServerToolUsage` -> `Generated.BetaServerToolUsage`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaSkill` -> `Generated.BetaSkill`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaSkillParams` -> `Generated.BetaSkillParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaSkillParamsType` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaSkillType` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaSkillVersion` -> `Generated.BetaSkillVersion`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaStopReason` -> `Generated.BetaStopReason`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaTextEditor20241022` -> `Generated.BetaTextEditor_20241022`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaTextEditor20250124` -> `Generated.BetaTextEditor_20250124`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaTextEditor20250429` -> `Generated.BetaTextEditor_20250429`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaTextEditor20250728` -> `Generated.BetaTextEditor_20250728`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaTextEditorCodeExecutionToolResultErrorCode` -> `Generated.BetaTextEditorCodeExecutionToolResultErrorCode`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaThinkingConfigDisabled` -> `Generated.BetaThinkingConfigDisabled`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaThinkingConfigEnabled` -> `Generated.BetaThinkingConfigEnabled`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaThinkingConfigParam` -> `Generated.BetaThinkingConfigParam`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaTool` -> `Generated.BetaTool`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaToolChoice` -> `Generated.BetaToolChoice`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaToolChoiceAny` -> `Generated.BetaToolChoiceAny`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaToolChoiceAuto` -> `Generated.BetaToolChoiceAuto`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaToolChoiceNone` -> `Generated.BetaToolChoiceNone`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaToolChoiceTool` -> `Generated.BetaToolChoiceTool`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaToolUsesKeep` -> `Generated.BetaToolUsesKeep`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaToolUsesTrigger` -> `Generated.BetaToolUsesTrigger`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaURLImageSource` -> `Generated.BetaURLImageSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaURLPDFSource` -> `Generated.BetaURLPDFSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaUploadFileV1FilesPostParams` -> `Generated.BetaUploadFileV1FilesPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaUploadFileV1FilesPostRequest` -> `Generated.BetaUploadFileV1FilesPostRequestFormData`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaUsage` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaUsageServiceTierEnum` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BetaUserLocation` -> `Generated.BetaUserLocation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaWebFetchTool20250910` -> `Generated.BetaWebFetchTool_20250910`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaWebFetchToolResultErrorCode` -> `Generated.BetaWebFetchToolResultErrorCode`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaWebSearchTool20250305` -> `Generated.BetaWebSearchTool_20250305`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaWebSearchToolResultErrorCode` -> `Generated.BetaWebSearchToolResultErrorCode`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BetaapiSchemasSkillsSkill` -> `Generated.Betaapi__schemas__skills__Skill`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BillingError` -> `Generated.BillingError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BodyCreateSkillV1SkillsPost` -> `Generated.Body_create_skill_v1_skills_post`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BodyCreateSkillVersionV1SkillsSkillIdVersionsPost` -> `Generated.Body_create_skill_version_v1_skills__skill_id__versions_post`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CacheControlEphemeral` -> `Generated.CacheControlEphemeral`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CacheControlEphemeralTtl` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CacheCreation` -> `Generated.CacheCreation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Client` -> `Generated.AnthropicClient`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ClientError` -> `Generated.AnthropicClientError`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CompletePostParams` -> `Generated.CompletePostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CompletionRequest` -> `Generated.CompletionRequest`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CompletionResponse` -> `Generated.CompletionResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ContentBlock` -> `Generated.ContentBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ContentBlockSource` -> `Generated.ContentBlockSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CountMessageTokensParams` -> `Generated.CountMessageTokensParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CountMessageTokensResponse` -> `Generated.CountMessageTokensResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessageBatchParams` -> `Generated.CreateMessageBatchParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessageParams` -> `Generated.CreateMessageParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessageParamsServiceTier` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateSkillResponse` -> `Generated.CreateSkillResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateSkillV1SkillsPostParams` -> `Generated.CreateSkillV1SkillsPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateSkillVersionResponse` -> `Generated.CreateSkillVersionResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateSkillVersionV1SkillsSkillIdVersionsPostParams` -> `Generated.CreateSkillVersionV1SkillsSkillIdVersionsPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.DeleteFileV1FilesFileIdDeleteParams` -> `Generated.DeleteFileV1FilesFileIdDeleteParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.DeleteMessageBatchResponse` -> `Generated.DeleteMessageBatchResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.DeleteSkillResponse` -> `Generated.DeleteSkillResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.DeleteSkillV1SkillsSkillIdDeleteParams` -> `Generated.DeleteSkillV1SkillsSkillIdDeleteParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.DeleteSkillVersionResponse` -> `Generated.DeleteSkillVersionResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.DeleteSkillVersionV1SkillsSkillIdVersionsVersionDeleteParams` -> `Generated.DeleteSkillVersionV1SkillsSkillIdVersionsVersionDeleteParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.DownloadFileV1FilesFileIdContentGetParams` -> `Generated.DownloadFileV1FilesFileIdContentGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ErrorResponse` -> `Generated.ErrorResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.FileDeleteResponse` -> `Generated.FileDeleteResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.FileListResponse` -> `Generated.FileListResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.FileMetadataSchema` -> `Generated.FileMetadataSchema`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GatewayTimeoutError` -> `Generated.GatewayTimeoutError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetFileMetadataV1FilesFileIdGetParams` -> `Generated.GetFileMetadataV1FilesFileIdGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetSkillResponse` -> `Generated.GetSkillResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetSkillV1SkillsSkillIdGetParams` -> `Generated.GetSkillV1SkillsSkillIdGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetSkillVersionResponse` -> `Generated.GetSkillVersionResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetSkillVersionV1SkillsSkillIdVersionsVersionGetParams` -> `Generated.GetSkillVersionV1SkillsSkillIdVersionsVersionGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.InputContentBlock` -> `Generated.InputContentBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.InputMessage` -> `Generated.InputMessage`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.InputMessageRole` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.InputSchema` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.InvalidRequestError` -> `Generated.InvalidRequestError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListFilesV1FilesGetParams` -> `Generated.ListFilesV1FilesGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListResponseMessageBatch` -> `Generated.ListResponse_MessageBatch_`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListResponseModelInfo` -> `Generated.ListResponse_ModelInfo_`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListSkillVersionsResponse` -> `Generated.ListSkillVersionsResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListSkillVersionsV1SkillsSkillIdVersionsGetParams` -> `Generated.ListSkillVersionsV1SkillsSkillIdVersionsGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListSkillsResponse` -> `Generated.ListSkillsResponse`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListSkillsV1SkillsGetParams` -> `Generated.ListSkillsV1SkillsGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Message` -> `Generated.Message`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.MessageBatch` -> `Generated.MessageBatch`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.MessageBatchIndividualRequestParams` -> `Generated.MessageBatchIndividualRequestParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.MessageBatchProcessingStatus` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.MessageBatchesCancelParams` -> `Generated.MessageBatchesCancelParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.MessageBatchesDeleteParams` -> `Generated.MessageBatchesDeleteParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.MessageBatchesListParams` -> `Generated.MessageBatchesListParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.MessageBatchesPostParams` -> `Generated.MessageBatchesPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.MessageBatchesResultsParams` -> `Generated.MessageBatchesResultsParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.MessageBatchesRetrieveParams` -> `Generated.MessageBatchesRetrieveParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.MessagesCountTokensPostParams` -> `Generated.MessagesCountTokensPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.MessagesPostParams` -> `Generated.MessagesPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Metadata` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.Model` -> `Generated.Model`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ModelInfo` -> `Generated.ModelInfo`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ModelsGetParams` -> `Generated.ModelsGetParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ModelsListParams` -> `Generated.ModelsListParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.NotFoundError` -> `Generated.NotFoundError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OverloadedError` -> `Generated.OverloadedError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PermissionError` -> `Generated.PermissionError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PlainTextSource` -> `Generated.PlainTextSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RateLimitError` -> `Generated.RateLimitError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestCharLocationCitation` -> `Generated.RequestCharLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestCitationsConfig` -> `Generated.RequestCitationsConfig`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestContentBlockLocationCitation` -> `Generated.RequestContentBlockLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestCounts` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.RequestDocumentBlock` -> `Generated.RequestDocumentBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestImageBlock` -> `Generated.RequestImageBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestPageLocationCitation` -> `Generated.RequestPageLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestRedactedThinkingBlock` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.RequestSearchResultBlock` -> `Generated.RequestSearchResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestSearchResultLocationCitation` -> `Generated.RequestSearchResultLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestServerToolUseBlock` -> `Generated.RequestServerToolUseBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestTextBlock` -> `Generated.RequestTextBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestThinkingBlock` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.RequestToolResultBlock` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.RequestToolUseBlock` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.RequestWebSearchResultBlock` -> `Generated.RequestWebSearchResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestWebSearchResultLocationCitation` -> `Generated.RequestWebSearchResultLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestWebSearchToolResultBlock` -> `Generated.RequestWebSearchToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestWebSearchToolResultError` -> `Generated.RequestWebSearchToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseCharLocationCitation` -> `Generated.ResponseCharLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseContentBlockLocationCitation` -> `Generated.ResponseContentBlockLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponsePageLocationCitation` -> `Generated.ResponsePageLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseRedactedThinkingBlock` -> `Generated.ResponseRedactedThinkingBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseSearchResultLocationCitation` -> `Generated.ResponseSearchResultLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseServerToolUseBlock` -> `Generated.ResponseServerToolUseBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseTextBlock` -> `Generated.ResponseTextBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseThinkingBlock` -> `Generated.ResponseThinkingBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseToolUseBlock` -> `Generated.ResponseToolUseBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseWebSearchResultBlock` -> `Generated.ResponseWebSearchResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseWebSearchResultLocationCitation` -> `Generated.ResponseWebSearchResultLocationCitation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseWebSearchToolResultBlock` -> `Generated.ResponseWebSearchToolResultBlock`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseWebSearchToolResultError` -> `Generated.ResponseWebSearchToolResultError`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ServerToolUsage` -> `Generated.ServerToolUsage`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Skill` -> `Generated.Skill`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.SkillVersion` -> `Generated.SkillVersion`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.StopReason` -> `Generated.StopReason`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.TextEditor20250124` -> `Generated.TextEditor_20250124`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.TextEditor20250429` -> `Generated.TextEditor_20250429`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.TextEditor20250728` -> `Generated.TextEditor_20250728`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ThinkingConfigDisabled` -> `Generated.ThinkingConfigDisabled`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ThinkingConfigEnabled` -> `Generated.ThinkingConfigEnabled`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ThinkingConfigParam` -> `Generated.ThinkingConfigParam`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Tool` -> `Generated.Tool`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ToolChoice` -> `Generated.ToolChoice`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ToolChoiceAny` -> `Generated.ToolChoiceAny`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ToolChoiceAuto` -> `Generated.ToolChoiceAuto`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ToolChoiceNone` -> `Generated.ToolChoiceNone`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ToolChoiceTool` -> `Generated.ToolChoiceTool`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.URLImageSource` -> `Generated.URLImageSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.URLPDFSource` -> `Generated.URLPDFSource`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.UploadFileV1FilesPostParams` -> `Generated.UploadFileV1FilesPostParams`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.UploadFileV1FilesPostRequest` -> `Generated.UploadFileV1FilesPostRequestFormData`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.UsageServiceTierEnum` -> `none`: Removed or inlined when the Anthropic specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.UserLocation` -> `Generated.UserLocation`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.WebSearchTool20250305` -> `Generated.WebSearchTool_20250305`: Regenerated in v4 under this name; update the identifier and re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.WebSearchToolResultErrorCode` -> `Generated.WebSearchToolResultErrorCode`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.make` -> `Generated.make`: Still generated in v4 from the current Anthropic specification; re-check the schema's Type/Encoded shape because the generated definition changed.
