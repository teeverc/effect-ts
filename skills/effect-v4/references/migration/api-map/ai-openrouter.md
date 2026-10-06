# v3 → v4 API map: @effect/ai-openrouter (4 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/ai-openrouter/Generated`

- `Generated.ActivityItem` -> `Generated.ActivityItem`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.AnnotationDetail` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesRequestProviderSort` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesRequestRoute` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesRequestServiceTier` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesRequestThinkingEnumType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesRequestToolChoiceEnumType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesResponse` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesResponseRole` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesResponseStopReason` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesResponseType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AnthropicMessagesResponseUsageServiceTier` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.AssistantMessage` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BadGatewayResponse` -> `Generated.BadGatewayResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BadGatewayResponseErrorData` -> `Generated.BadGatewayResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BadRequestResponse` -> `Generated.BadRequestResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BadRequestResponseErrorData` -> `Generated.BadRequestResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BigNumberUnion` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BulkAssignKeysToGuardrail200` -> `Generated.BulkAssignKeysToGuardrail200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BulkAssignKeysToGuardrailRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BulkAssignMembersToGuardrail200` -> `Generated.BulkAssignMembersToGuardrail200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BulkAssignMembersToGuardrailRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BulkUnassignKeysFromGuardrail200` -> `Generated.BulkUnassignKeysFromGuardrail200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BulkUnassignKeysFromGuardrailRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.BulkUnassignMembersFromGuardrail200` -> `Generated.BulkUnassignMembersFromGuardrail200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.BulkUnassignMembersFromGuardrailRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CacheControlEphemeral` -> `Generated.ChatContentCacheControl`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatCompletionFinishReason` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ChatError` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ChatGenerationParams` -> `Generated.ChatRequest`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatGenerationParamsProviderEnumDataCollectionEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ChatGenerationParamsReasoningEffortEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ChatGenerationParamsRouteEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ChatGenerationTokenUsage` -> `Generated.ChatUsage`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatMessageContentItem` -> `Generated.ChatContentItems`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatMessageContentItemAudio` -> `Generated.ChatContentAudio`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatMessageContentItemCacheControl` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ChatMessageContentItemCacheControlTtl` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ChatMessageContentItemImage` -> `Generated.ChatContentImage`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatMessageContentItemImageImageUrlDetail` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ChatMessageContentItemText` -> `Generated.ChatContentText`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatMessageContentItemVideo` -> `Generated.ChatContentVideo`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatMessageTokenLogprob` -> `Generated.ChatTokenLogprob`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatMessageTokenLogprobs` -> `Generated.ChatTokenLogprobs`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatMessageToolCall` -> `Generated.ChatToolCall`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatResponse` -> `Generated.ChatResult`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ChatResponseChoice` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ChatStreamOptions` -> `Generated.ChatStreamOptions`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Client` -> `Generated.OpenRouterClient`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ClientError` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CompletionChoice` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CompletionCreateParams` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CompletionFinishReason` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CompletionFinishReasonEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CompletionLogprobs` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CompletionResponse` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CompletionUsage` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateAuthKeysCode200` -> `Generated.CreateAuthKeysCode200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateAuthKeysCodeRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateAuthKeysCodeRequestCodeChallengeMethod` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateChargeRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateChargeRequestChainId` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateCoinbaseCharge200` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateEmbeddings200` -> `Generated.CreateEmbeddings200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateEmbeddings200Object` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateEmbeddingsRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateEmbeddingsRequestEncodingFormat` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateGuardrail201` -> `Generated.CreateGuardrail201`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateGuardrail201DataResetInterval` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateGuardrailRequest` -> `Generated.CreateGuardrailRequest`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateGuardrailRequestResetInterval` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateKeys201` -> `Generated.CreateKeys201`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateKeysRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateKeysRequestLimitReset` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateMessages400` -> `Generated.CreateMessages400`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessages400Type` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateMessages401` -> `Generated.CreateMessages401`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessages401Type` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateMessages403` -> `Generated.CreateMessages403`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessages403Type` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateMessages404` -> `Generated.CreateMessages404`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessages404Type` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateMessages429` -> `Generated.CreateMessages429`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessages429Type` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateMessages500` -> `Generated.CreateMessages500`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessages500Type` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateMessages503` -> `Generated.CreateMessages503`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessages503Type` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.CreateMessages529` -> `Generated.CreateMessages529`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.CreateMessages529Type` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.DataCollection` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.DefaultParameters` -> `Generated.DefaultParameters`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.DeleteGuardrail200` -> `Generated.DeleteGuardrail200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.DeleteKeys200` -> `Generated.DeleteKeys200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.EdgeNetworkTimeoutResponse` -> `Generated.EdgeNetworkTimeoutResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.EdgeNetworkTimeoutResponseErrorData` -> `Generated.EdgeNetworkTimeoutResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.EndpointStatus` -> `Generated.EndpointStatus`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ExchangeAuthCodeForAPIKey200` -> `Generated.ExchangeAuthCodeForAPIKey200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ExchangeAuthCodeForAPIKeyRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ExchangeAuthCodeForAPIKeyRequestCodeChallengeMethod` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.FileAnnotationDetail` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.FileCitation` -> `Generated.FileCitation`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.FileCitationType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.FilePath` -> `Generated.FilePath`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.FilePathType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ForbiddenResponse` -> `Generated.ForbiddenResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ForbiddenResponseErrorData` -> `Generated.ForbiddenResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetCredits200` -> `Generated.GetCredits200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetCurrentKey200` -> `Generated.GetCurrentKey200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetGeneration200` -> `Generated.GetGeneration200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetGeneration200DataApiType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.GetGenerationParams` -> `Generated.GetGenerationParams`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetGuardrail200` -> `Generated.GetGuardrail200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetGuardrail200DataResetInterval` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.GetKey200` -> `Generated.GetKey200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetModelsParams` -> `Generated.GetModelsParams`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetModelsParamsCategory` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.GetUserActivity200` -> `Generated.GetUserActivity200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.GetUserActivityParams` -> `Generated.GetUserActivityParams`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ImageGenerationStatus` -> `Generated.ImageGenerationStatus`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.InputModality` -> `Generated.InputModality`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.InternalServerResponse` -> `Generated.InternalServerResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.InternalServerResponseErrorData` -> `Generated.InternalServerResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.JSONSchemaConfig` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.List200` -> `Generated.List200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListEndpoints200` -> `Generated.ListEndpoints200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListEndpointsResponse` -> `Generated.ListEndpointsResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListEndpointsResponseArchitecture` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ListEndpointsResponseArchitectureEnumInstructType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ListEndpointsZdr200` -> `Generated.ListEndpointsZdr200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListGuardrailKeyAssignments200` -> `Generated.ListGuardrailKeyAssignments200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListGuardrailKeyAssignmentsParams` -> `Generated.ListGuardrailKeyAssignmentsParams`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListGuardrailMemberAssignments200` -> `Generated.ListGuardrailMemberAssignments200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListGuardrailMemberAssignmentsParams` -> `Generated.ListGuardrailMemberAssignmentsParams`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListGuardrails200` -> `Generated.ListGuardrails200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListGuardrailsParams` -> `Generated.ListGuardrailsParams`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListKeyAssignments200` -> `Generated.ListKeyAssignments200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListKeyAssignmentsParams` -> `Generated.ListKeyAssignmentsParams`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListMemberAssignments200` -> `Generated.ListMemberAssignments200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListMemberAssignmentsParams` -> `Generated.ListMemberAssignmentsParams`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListParams` -> `Generated.ListParams`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ListProviders200` -> `Generated.ListProviders200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Message` -> `Generated.ChatMessages`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.Model` -> `Generated.Model`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ModelArchitecture` -> `Generated.ModelArchitecture`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ModelArchitectureInstructType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ModelGroup` -> `Generated.ModelGroup`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ModelName` -> `Generated.ModelName`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ModelsCountResponse` -> `Generated.ModelsCountResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ModelsListResponse` -> `Generated.ModelsListResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ModelsListResponseData` -> `Generated.ModelsListResponseData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.NamedToolChoice` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.NotFoundResponse` -> `Generated.NotFoundResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.NotFoundResponseErrorData` -> `Generated.NotFoundResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OpenAIResponsesAnnotation` -> `Generated.OpenAIResponsesAnnotation`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OpenAIResponsesIncludable` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesIncompleteDetails` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesIncompleteDetailsReason` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesInput` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesPrompt` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesReasoningConfig` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesReasoningEffort` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesRefusalContent` -> `Generated.OpenAIResponsesRefusalContent`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OpenAIResponsesRefusalContentType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesResponseStatus` -> `Generated.OpenAIResponsesResponseStatus`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OpenAIResponsesServiceTier` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesToolChoice` -> `Generated.OpenAIResponsesToolChoice`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OpenAIResponsesToolChoiceEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesToolChoiceEnumType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesToolChoiceEnumTypeEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenAIResponsesTruncation` -> `Generated.OpenAIResponsesTruncation`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OpenAIResponsesUsage` -> `Generated.OpenAIResponsesUsage`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OpenResponsesEasyInputMessage` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesEasyInputMessageRoleEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesEasyInputMessageType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesFunctionCallOutput` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesFunctionCallOutputType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesFunctionToolCall` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesFunctionToolCallType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesInput` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesInputMessageItem` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesInputMessageItemRoleEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesInputMessageItemType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesNonStreamingResponse` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesNonStreamingResponseObject` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesReasoning` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesReasoningConfig` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesReasoningFormat` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesReasoningStatusEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesReasoningType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesRequestMetadata` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesRequestRoute` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesRequestServiceTier` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesRequestTruncation` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesRequestTruncationEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesResponseText` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesResponseTextVerbosity` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesWebSearch20250826Tool` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesWebSearch20250826ToolType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesWebSearchPreview20250311Tool` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesWebSearchPreview20250311ToolType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesWebSearchPreviewTool` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesWebSearchPreviewToolType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesWebSearchTool` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenResponsesWebSearchToolType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenRouterAnthropicMessageParam` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OpenRouterAnthropicMessageParamRole` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputItemFileSearchCall` -> `Generated.OutputItemFileSearchCall`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OutputItemFileSearchCallType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputItemFunctionCall` -> `Generated.OutputItemFunctionCall`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OutputItemFunctionCallStatusEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputItemFunctionCallType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputItemImageGenerationCall` -> `Generated.OutputItemImageGenerationCall`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OutputItemImageGenerationCallType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputItemReasoning` -> `Generated.OutputItemReasoning`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OutputItemReasoningStatusEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputItemReasoningType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputItemWebSearchCall` -> `Generated.OutputItemWebSearchCall`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OutputItemWebSearchCallType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputMessage` -> `Generated.OutputMessage`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.OutputMessageRole` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputMessageStatusEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputMessageType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.OutputModality` -> `Generated.OutputModality`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PDFParserEngine` -> `Generated.PDFParserEngine`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PDFParserOptions` -> `Generated.PDFParserOptions`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Parameter` -> `Generated.Parameter`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PayloadTooLargeResponse` -> `Generated.PayloadTooLargeResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PayloadTooLargeResponseErrorData` -> `Generated.PayloadTooLargeResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PaymentRequiredResponse` -> `Generated.PaymentRequiredResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PaymentRequiredResponseErrorData` -> `Generated.PaymentRequiredResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PerRequestLimits` -> `Generated.PerRequestLimits`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PercentileLatencyCutoffs` -> `Generated.PercentileLatencyCutoffs`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PercentileStats` -> `Generated.PercentileStats`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PercentileThroughputCutoffs` -> `Generated.PercentileThroughputCutoffs`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PreferredMaxLatency` -> `Generated.PreferredMaxLatency`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PreferredMinThroughput` -> `Generated.PreferredMinThroughput`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ProviderName` -> `Generated.ProviderName`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ProviderOverloadedResponse` -> `Generated.ProviderOverloadedResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ProviderOverloadedResponseErrorData` -> `Generated.ProviderOverloadedResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ProviderPreferences` -> `Generated.ProviderPreferences`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ProviderPreferencesSort` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ProviderSort` -> `Generated.ProviderSort`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ProviderSortConfig` -> `Generated.ProviderSortConfig`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ProviderSortConfigPartitionEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ProviderSortUnion` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.PublicEndpoint` -> `Generated.PublicEndpoint`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.PublicEndpointQuantization` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.PublicEndpointQuantizationEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.PublicEndpointThroughputLast30M` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.PublicPricing` -> `Generated.PublicPricing`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.Quantization` -> `Generated.Quantization`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ReasoningDetail` -> `Generated.ReasoningDetailUnion`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ReasoningDetailEncrypted` -> `Generated.ReasoningDetailEncrypted`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ReasoningDetailSummary` -> `Generated.ReasoningDetailSummary`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ReasoningDetailText` -> `Generated.ReasoningDetailText`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ReasoningSummaryText` -> `Generated.ReasoningSummaryText`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ReasoningSummaryTextType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ReasoningSummaryVerbosity` -> `Generated.ReasoningSummaryVerbosity`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ReasoningTextContent` -> `Generated.ReasoningTextContent`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ReasoningTextContentType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.RequestTimeoutResponse` -> `Generated.RequestTimeoutResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.RequestTimeoutResponseErrorData` -> `Generated.RequestTimeoutResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseFormatJSONSchema` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseFormatTextConfig` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseFormatTextGrammar` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputAudio` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputAudioInputAudioFormat` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputAudioType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputFile` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputFileType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputImage` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputImageDetail` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputImageType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputText` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputTextType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputVideo` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseInputVideoType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseOutputText` -> `Generated.ResponseOutputText`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponseOutputTextType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseTextConfig` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponseTextConfigVerbosity` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesErrorField` -> `Generated.ResponsesErrorField`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ResponsesErrorFieldCode` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesFormatJSONObject` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesFormatJSONObjectType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesFormatText` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesFormatTextJSONSchemaConfig` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesFormatTextJSONSchemaConfigType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesFormatTextType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesImageGenerationCall` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesImageGenerationCallType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputItemFileSearchCall` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputItemFileSearchCallType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputItemFunctionCall` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputItemFunctionCallStatusEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputItemFunctionCallType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputItemReasoning` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputItemReasoningFormat` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputItemReasoningStatusEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputItemReasoningType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputMessage` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputMessageRole` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputMessageStatusEnum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputMessageType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesOutputModality` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesSearchContextSize` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesWebSearchCallOutput` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesWebSearchCallOutputType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesWebSearchUserLocation` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ResponsesWebSearchUserLocationType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.Schema0` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.Schema1` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.Schema2` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.Schema3` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.Schema4` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.Schema4Enum` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.Schema5` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.Schema6` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.ServiceUnavailableResponse` -> `Generated.ServiceUnavailableResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ServiceUnavailableResponseErrorData` -> `Generated.ServiceUnavailableResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.SystemMessage` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.TooManyRequestsResponse` -> `Generated.TooManyRequestsResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.TooManyRequestsResponseErrorData` -> `Generated.TooManyRequestsResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ToolCallStatus` -> `Generated.ToolCallStatus`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.ToolChoiceOption` -> `Generated.ChatToolChoice`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ToolDefinitionJson` -> `Generated.ChatFunctionTool`: Renamed when the OpenRouter client was regenerated from the current specification; re-check the replacement schema's Type/Encoded shape.

- `Generated.ToolResponseMessage` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.TopProviderInfo` -> `Generated.TopProviderInfo`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.URLCitation` -> `Generated.URLCitation`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.URLCitationAnnotationDetail` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.URLCitationType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.UnauthorizedResponse` -> `Generated.UnauthorizedResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.UnauthorizedResponseErrorData` -> `Generated.UnauthorizedResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.UnprocessableEntityResponse` -> `Generated.UnprocessableEntityResponse`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.UnprocessableEntityResponseErrorData` -> `Generated.UnprocessableEntityResponseErrorData`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.UpdateGuardrail200` -> `Generated.UpdateGuardrail200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.UpdateGuardrail200DataResetInterval` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.UpdateGuardrailRequest` -> `Generated.UpdateGuardrailRequest`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.UpdateGuardrailRequestResetInterval` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.UpdateKeys200` -> `Generated.UpdateKeys200`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.UpdateKeysRequest` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.UpdateKeysRequestLimitReset` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.UserMessage` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.WebSearchEngine` -> `Generated.WebSearchEngine`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.WebSearchPreviewToolUserLocation` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.WebSearchPreviewToolUserLocationType` -> `none`: Removed when the OpenRouter specification client was regenerated; use the current Generated request/response schema for the relevant endpoint instead of this old snapshot helper.

- `Generated.WebSearchStatus` -> `Generated.WebSearchStatus`: Still generated in v4 from the current OpenRouter specification; re-check the schema's Type/Encoded shape because the generated definition changed.

- `Generated.make` -> `Generated.make`: Still exported in v4, but the regenerated OpenRouter client has different operations and request/response schemas; update call sites to the current generated service.

## `@effect/ai-openrouter/OpenRouterClient`

- `OpenRouterClient.ChatStreamingChoice` -> `Generated.ChatStreamChoice`: The client-local streaming choice schema moved into the regenerated OpenRouter schema surface and changed shape.

- `OpenRouterClient.ChatStreamingMessageChunk` -> `Generated.ChatStreamDelta`: The client-local streaming message delta moved into the regenerated OpenRouter schema surface and changed shape.

- `OpenRouterClient.ChatStreamingMessageToolCall` -> `Generated.ChatStreamToolCall`: The client-local streaming tool-call delta moved into the regenerated OpenRouter schema surface and changed shape.

- `OpenRouterClient.ChatStreamingResponseChunk` -> `OpenRouterClient.ChatStreamingResponseChunkData`: The standalone streaming chunk schema was replaced by the decoded data type from Generated.ChatStreamingResponse.

- `OpenRouterClient.Service` -> `OpenRouterClient.Service`: Still exported in v4; adapt to the regenerated client, revised request and response schemas, and the new streaming result tuple. Custom service implementations and mocks must also provide createDecisions, returning an Effect of [decoded DecisionsResponse, HttpClientResponse] with AiError failures.

## `@effect/ai-openrouter/OpenRouterConfig`

- `OpenRouterConfig.OpenRouterConfig` -> `OpenRouterConfig.OpenRouterConfig`: Still exported in v4; update imports to the v4 package and use the revised Context.Service-based configuration service.

- `OpenRouterConfig.OpenRouterConfig.Service` -> `OpenRouterConfig.OpenRouterConfig.Service`: Still exported in v4; update imports to the v4 package and use the revised Context.Service-based configuration service.

## `@effect/ai-openrouter/OpenRouterLanguageModel`

- `OpenRouterLanguageModel.Config` -> `OpenRouterLanguageModel.Config`: Still exported in v4; update imports and adapt to the regenerated chat request schema and revised Context.Service configuration.

- `OpenRouterLanguageModel.Config.Service` -> `OpenRouterLanguageModel.Config.Service`: Still exported in v4; update imports and adapt to the regenerated chat request schema and revised Context.Service configuration.

- `OpenRouterLanguageModel.OpenRouterReasoningInfo` -> `OpenRouterLanguageModel.ReasoningDetails`: The bespoke reasoning-info union was replaced by the provider's raw reasoning-details array, preserved through Prompt options and Response metadata.
