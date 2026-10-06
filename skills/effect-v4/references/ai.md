# Effect AI (`effect/ai` + provider packages)

Use this guide for LLM work in Effect 4.0.x: text/object generation,
streaming, tools, chat sessions, embeddings, classification (DecisionModel),
tool approvals and MCP servers. All of `effect/ai` and the provider packages
are `@stability unstable`.

Official examples (read these first):
`references/examples/71_ai/10_language-model.ts` (providers, ExecutionPlan
fallback, generateText/generateObject/streamText),
`references/examples/71_ai/20_tools.ts` (Tool, Toolkit, handlers, provider tools),
`references/examples/71_ai/30_chat.ts` (Chat sessions, agent loop).

## Packages

| Import | Contents |
| --- | --- |
| `effect/ai` | `LanguageModel`, `Prompt`, `Response`, `Tool`, `Toolkit`, `Chat`, `EmbeddingModel`, `DecisionModel`, `Decision`, `Model`, `AiError`, `IdGenerator`, `Tokenizer`, `Telemetry`, `McpServer`, `McpSchema`, `McpProtocol` |
| `@effect/ai-openai` | `OpenAiClient`, `OpenAiLanguageModel`, `OpenAiEmbeddingModel`, `OpenAiTool` (web search, ...), `OpenAiConfig` |
| `@effect/ai-anthropic` | `AnthropicClient`, `AnthropicLanguageModel`, `AnthropicTool`, `AnthropicConfig` |
| `@effect/ai-openai-compat` | `OpenAiClient` (with `apiUrl`), `OpenAiLanguageModel`, `OpenAiEmbeddingModel` for OpenAI-compatible servers |
| `@effect/ai-openrouter` | `OpenRouterClient`, `OpenRouterLanguageModel`, `OpenRouterDecisionModel` |
| `@effect/ai-typesafe` | `TypeSafeClient`, `TypeSafeDecisionModel` (DecisionModel provider) |
| `@effect/ai-cloudflare` | `CloudflareClient`, `CloudflareDecisionModel` |

Provider packages share the `effect` version and need an `HttpClient`
(`FetchHttpClient.layer` from `effect/http`, or a platform client).

## Provider setup

```ts
import { AnthropicClient, AnthropicLanguageModel } from "@effect/ai-anthropic"
import { Config, Effect, Layer } from "effect"
import { LanguageModel } from "effect/ai"
import { FetchHttpClient } from "effect/http"

const AnthropicLive = AnthropicClient.layerConfig({
  apiKey: Config.Redacted("ANTHROPIC_API_KEY")   // also: apiUrl, ...
}).pipe(Layer.provide(FetchHttpClient.layer))

// `X.model(name, config?)` is a Model (usable as a Layer) providing LanguageModel
const Claude = AnthropicLanguageModel.model("claude-opus-4-6")

const program = LanguageModel.generateText({ prompt: "Say hi" }).pipe(
  Effect.map((r) => r.text),
  Effect.provide(Claude),
  Effect.provide(AnthropicLive)
)
```

- Inside a service layer, `yield* AnthropicLanguageModel.model("...").captureRequirements`
  moves the client requirement into the enclosing layer and returns a provider-free Layer you can
  `Effect.provide` per call (see `10_language-model.ts`).
- `XLanguageModel.withConfigOverride({ ... })` overrides request config
  (temperature, etc.) for one effect.
- `Model.ProviderName` / `Model.ModelName` are services naming the active model.
- Fallback across providers: `ExecutionPlan.make({ provide: modelA, attempts: 3 }, { provide: modelB })`
  + `Effect.withExecutionPlan(plan)`.

## LanguageModel

```ts
import { Effect, Schema, Stream } from "effect"
import { LanguageModel, type Response } from "effect/ai"

const Plan = Schema.Struct({ title: Schema.String, steps: Schema.Array(Schema.String) })

const demo = Effect.gen(function*() {
  const text = yield* LanguageModel.generateText({ prompt: "Summarize Effect in 1 line" })
  text.text; text.finishReason; text.usage.outputTokens.total; text.toolCalls; text.toolResults

  const obj = yield* LanguageModel.generateObject({
    objectName: "plan", prompt: "Plan a launch", schema: Plan
  })
  obj.value // decoded with Plan; decode failure -> AiError

  const deltas = LanguageModel.streamText({ prompt: "Tell a story" }).pipe(
    Stream.filter((p): p is Response.TextDeltaPart => p.type === "text-delta"),
    Stream.map((p) => p.delta)
  )
  return deltas
})
```

Options (`GenerateTextOptions`): `prompt` (string, messages array, or `Prompt`),
`toolkit`, `toolChoice` (`"auto" | "none" | "required" | { tool } | { mode, oneOf }`),
`concurrency` (tool resolution), `disableToolCallResolution`.
Prompts: `Prompt.make(...)`, `Prompt.empty.pipe(Prompt.setSystem("..."))`,
`Prompt.userMessage`, `Prompt.concat`, raw `{ role, content }` arrays.

## Tools and toolkits

```ts
import { Effect, Schema } from "effect"
import { LanguageModel, Tool, Toolkit } from "effect/ai"

const GetWeather = Tool.make("GetWeather", {
  description: "Current weather for a city",
  parameters: Schema.Struct({ city: Schema.String.annotate({ description: "City name" }) }),
  success: Schema.Struct({ tempC: Schema.Finite }),
  failure: Schema.String,         // optional
  failureMode: "error"            // or "return": handler failures go back to the model
})
const Now = Tool.make("Now", { success: Schema.String })   // no params = Tool.EmptyParams

const Tools = Toolkit.make(GetWeather, Now)                // Toolkit.merge(a, b) to combine

const ToolsLive = Tools.toLayer({                          // or toLayer(Effect.gen(...)) for deps
  GetWeather: ({ city }) => Effect.succeed({ tempC: city.length }),
  Now: () => Effect.succeed(new Date(0).toISOString())
})

const ask = Effect.gen(function*() {
  const toolkit = yield* Tools                             // resolves handlers
  const res = yield* LanguageModel.generateText({ prompt: "Weather in Oslo?", toolkit })
  return res.text
}).pipe(Effect.provide(ToolsLive))
```

- One `generateText` call runs **one** model round: tool calls are resolved and
  results returned, but the model is not re-invoked. Loop (usually with `Chat`)
  until `response.toolCalls.length === 0` for agents (`30_chat.ts`).
- Tool annotations: `Tool.Readonly`, `Tool.Destructive`, `Tool.Idempotent`,
  `Tool.OpenWorld`, `Tool.Title`, `Tool.Strict` (apply with `tool.annotate(Tool.Readonly, true)`).
- Provider-executed tools: `OpenAiTool.WebSearch({...})`, `AnthropicTool.*`, or
  `Tool.providerDefined(...)`; they need no handler in `toLayer`.

### Dynamic tools

When the schema is only known at runtime (MCP, config, plugins):

```ts
import { Schema } from "effect"
import { Tool } from "effect/ai"

const Typed = Tool.dynamic("Calculator", {
  parameters: Schema.Struct({ a: Schema.Number, b: Schema.Number }),
  success: Schema.Number
})
const Raw = Tool.dynamic("Search", {          // JSON Schema: handler gets `unknown`, no validation
  description: "Remote search",
  parameters: { type: "object", properties: { q: { type: "string" } }, required: ["q"] }
})
```

### Tool approvals

```ts
import { Schema } from "effect"
import { Prompt, Tool } from "effect/ai"

const DeleteFile = Tool.make("DeleteFile", {
  parameters: Schema.Struct({ path: Schema.String }),
  needsApproval: true            // or (params, { toolCallId, messages }) => boolean | Effect<boolean>
})
const Strict = DeleteFile.setNeedsApproval(({ path }) => path.startsWith("/etc"))

// 1st call: the response contains a "tool-approval-request" part (approvalId) instead of a result.
// 2nd call: send the decision; approved calls run, denied ones are reported to the model.
const approve = (approvalId: string) =>
  Prompt.toolMessage({ content: [Prompt.toolApprovalResponsePart({ approvalId, approved: true })] })
// chat.generateText({ toolkit, prompt: [approve(part.approvalId)] })
```

## Chat (stateful sessions)

- `Chat.empty`, `Chat.fromPrompt(prompt)`, `Chat.fromJson(json)`,
  `Chat.fromExport(data)`; on a session: `generateText`, `generateObject`,
  `streamText`, `history` (a `Ref<Prompt>`), `export`, `exportJson`.
- Persistence: `Chat.layerPersisted({ storeId })` (needs
  `effect/persistence` backing, e.g. `Persistence.layerBackingMemory`) then
  `const chat = yield* (yield* Chat.Persistence).getOrCreate(chatId)`.

## EmbeddingModel

```ts
import { OpenAiClient, OpenAiEmbeddingModel } from "@effect/ai-openai"
import { Effect } from "effect"
import { EmbeddingModel } from "effect/ai"

const embed = Effect.gen(function*() {
  const model = yield* EmbeddingModel.EmbeddingModel
  const one = yield* model.embed("hello")                    // { vector }
  const many = yield* model.embedMany(["a", "b"])            // { embeddings: [{ vector }], usage }
  return [one.vector, many.embeddings.length]
}).pipe(Effect.provide(OpenAiEmbeddingModel.model("text-embedding-3-small", { dimensions: 1536 })))
// also provides EmbeddingModel.Dimensions; requires OpenAiClient
```

Concurrent `embed` calls are batched by `EmbeddingModel.make`.

## DecisionModel (classification, rating, probability)

```ts
import { Effect, Schema } from "effect"
import { Decision, DecisionModel } from "effect/ai"

const Triage = Decision.make({
  input: Schema.Struct({ subject: Schema.String }),
  decisions: {
    team: Decision.classify({ instructions: "Which team", criteria: { billing: "payments", tech: "bugs" } }),
    mood: Decision.rate({ instructions: "How upset", criteria: ["calm", "frustrated", "angry"] }),
    urgent: Decision.probability({ instructions: "Time-sensitive", criteria: { false: "no", true: "yes" } })
  }
})

const triage = Effect.gen(function*() {
  const { answers } = yield* DecisionModel.decide(Triage, { input: { subject: "Charged twice" } })
  answers.team.label; answers.team.confidence; answers.mood.rating; answers.urgent.probability
})
// Providers: OpenRouterDecisionModel.model(...), TypeSafeDecisionModel.model("jev-latest"), CloudflareDecisionModel
```

## MCP server

```ts
import { NodeRuntime, NodeStdio } from "@effect/platform-node"
import { Effect, Layer, Logger, Schema } from "effect"
import { McpProtocol, McpServer, Tool, Toolkit } from "effect/ai"

const Echo = Tool.make("Echo", { parameters: Schema.Struct({ message: Schema.String }), success: Schema.String })
const Tools = Toolkit.make(Echo)

const Server = Layer.mergeAll(
  McpServer.toolkit(Tools).pipe(Layer.provideMerge(Tools.toLayer({ Echo: ({ message }) => Effect.succeed(message) }))),
  McpServer.resource({ uri: "file:///readme.md", name: "Readme", content: Effect.succeed("# Hi") }),
  McpServer.prompt({ name: "Explain", parameters: { topic: Schema.String }, content: ({ topic }) => Effect.succeed(`Explain ${topic}`) })
).pipe(
  Layer.provide(McpServer.layerStdio({ name: "demo", version: "1.0.0", protocols: [McpProtocol.v2025_06_18] })),
  Layer.provide(NodeStdio.layer),
  Layer.provideMerge(Layer.succeed(Logger.LogToStderr, true))   // keep stdout clean for the protocol
)
Layer.launch(Server).pipe(NodeRuntime.runMain)
```

- `protocols` is a required non-empty ordered list of adapters:
  `McpProtocol.v2024_11_05`, `v2025_03_26`, `v2025_06_18`, `v2025_11_25`, `v2026_07_28`.
- HTTP: `McpServer.layerHttp({ name, version, path: "/mcp", protocols, allowedOrigins? })`
  (requires `HttpRouter`); custom transport: `McpServer.layer(...)` + `RpcServer.Protocol`.
- Templated resources: ``McpServer.resource`file:///docs/${McpSchema.param("id", Schema.NumberFromString)}`({ name, content })``.
- Inside handlers: `McpServer.elicit({ message, schema })` asks the client for input.
- Full guide: upstream `packages/effect/MCP.md`.

## Testing

Mock the model with `LanguageModel.make({ generateText, streamText })`
returning encoded response parts, provided via
`Effect.provideServiceEffect(LanguageModel.LanguageModel, LanguageModel.make({...}))`;
similarly `EmbeddingModel.make({ embedMany })` and `DecisionModel.make({ decide })`.

```ts
import { Effect, Stream } from "effect"
import { LanguageModel } from "effect/ai"

const withFakeModel = Effect.provideServiceEffect(
  LanguageModel.LanguageModel,
  LanguageModel.make({
    generateText: () => Effect.succeed([{ type: "text", text: "stubbed" }]),
    streamText: () => Stream.empty
  })
)
```

## Errors

All operations fail with `AiError.AiError` (`_tag: "AiError"`) carrying a
semantic `reason` (`RateLimitError`, `AuthenticationError`, `InvalidOutputError`,
`StructuredOutputError`, `ToolNotFoundError`, `ToolParameterValidationError`,
`NetworkError`, ...). `AiError.AiErrorReason` is a Schema - embed it in your own
`Schema.TaggedError` and map with `Effect.mapError` (see examples).

## Pitfalls

- Expecting `generateText` to run a full multi-step agent loop - loop yourself.
- Forgetting the toolkit handler layer (`Tools.toLayer(...)`) - `yield* Tools` needs it.
- Logging prompts/responses with secrets/PII; keys belong in `Config.Redacted`.
- Writing to stdout in a stdio MCP server.
- Mismatched versions between `effect` and `@effect/ai-*`.

## v3 -> v4

- `@effect/ai` -> `effect/ai` (providers stay separate: `@effect/ai-*`).
- `LanguageModel.Service`, `Chat.Service`, `EmbeddingModel.Service` ->
  `LanguageModel.LanguageModel`, `Chat.Chat`, `EmbeddingModel.EmbeddingModel`
  service keys (no longer classes); build with `X.make(...)`.
- `AiError` is one error (`_tag: "AiError"`) wrapping a semantic `reason`
  instead of a union of tagged errors (`HttpRequestError`, `MalformedOutput`, ...).
- See `references/migration/api-map/ai.md`, `ai-openai.md`, `ai-anthropic.md`,
  `ai-openrouter.md`.
