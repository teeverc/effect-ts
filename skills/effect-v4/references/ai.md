# Effect AI (LLM Integrations, Planning, Tool Use)

Use this guide when tasks involve Effect AI integrations, structured planning, or tool-enabled LLM workflows.

## Mental model

- Treat model interaction as an effectful boundary: decode inputs, run model/tool flow, decode outputs.
- Keep prompts, tool contracts, and validation schemas explicit and versioned.
- Separate planning from execution: produce a typed plan first, then execute steps with controlled effects.
- Prefer deterministic tool contracts and schema validation over free-form parsing.

## Patterns

- Define request/response schemas for model IO at boundaries.
- Represent tool calls as typed services so they can be mocked in tests.
- Add retries/timeouts only where idempotency is clear.
- Record structured logs and spans for model call inputs/outputs (with redaction for sensitive fields).
- Fail fast on schema decode errors; do not silently coerce malformed model output.

## Agent workflow

1. Define the shape of model input/output and tool arguments using schemas.
2. Build a small interpreter layer for model calls and tool execution.
3. Add guardrails: timeout, retry policy, and explicit error mapping.
4. Keep execution pure from the caller perspective: return `Effect<A, E, R>`.
5. Cover with tests using test services and deterministic fixtures.

## Pitfalls

- Embedding tool side effects directly inside prompt composition.
- Accepting model output without schema checks.
- Mixing transient transport failures with domain-level model failures.
- Logging full prompts/responses that contain secrets or PII.
- Hard-coding provider-specific behavior into domain services.

## Docs

- `https://effect.website/docs/ai/introduction/`
- `https://effect.website/docs/ai/getting-started/`
- `https://effect.website/docs/ai/planning-llm-interactions/`
- `https://effect.website/docs/ai/tool-use/`
