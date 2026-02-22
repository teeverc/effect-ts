# Docs Index (`llms.txt` -> local references)

Use this guide when you need to quickly route an Effect task from official docs topics to the most relevant local skill reference.

## Canonical sources

- `https://effect.website/llms.txt` (topic index for LLM workflows)
- `https://effect.website/docs` (full docs)
- `https://effect.website/docs/additional-resources/api-reference` (API reference)
- `https://tim-smart.github.io/effect-io-ai/` (concise API list)

## Topic mapping

- Configuration, ConfigProvider -> `configuration.md`, `configuration-advanced.md`
- Runtime, Running Effects -> `runtime-execution.md`
- Expected/Unexpected Errors, Retrying, Sandboxing -> `error-management.md`, `error-tooling.md`, `scheduling-retry.md`
- Layers, Services, Default Services -> `dependency-management.md`, `layer-patterns.md`
- Scope and resources -> `resource-management.md`
- Fibers, Queue, PubSub, concurrency basics -> `concurrency.md`, `concurrency-advanced.md`, `streams-queues-stm.md`
- Scheduling (repetition/backoff/cron) -> `scheduling.md`, `scheduling-retry.md`
- Schema basics/advanced/transformations -> `schema.md`
- Logging, metrics, tracing, supervisor -> `observability.md`, `observability-examples.md`, `observability-wiring.md`
- Platform (Command, FileSystem, KeyValueStore, Path, PlatformLogger, Terminal) -> `platform-primitives.md`
- Streams + Sink -> `streams-queues-stm.md`, `sink.md`
- AI intro, planning, tool use -> `ai.md`
- Data types (Option/Either/Chunk/Duration/etc.) -> `core-usage.md`, `data-types-advanced.md`
- Behaviour + traits (Equivalence, Order, Equal, Hash) -> `behavior-traits.md`
- TestClock/testing services -> `testing.md`, `testing-stack.md`
- Promise migration and comparisons -> `migration-async.md`
- Version mismatch and debugging -> `versioning.md`, `troubleshooting.md`, `exit-cause.md`
- Bundle-size constrained apps (`Micro`) -> `micro.md`

## Agent routing checklist

- Start from task intent (error handling, schema, platform IO, streaming, AI, etc.).
- Open one primary guide and one adjacent guide (for cross-cutting concerns like testing or runtime).
- Confirm APIs against official docs/API reference before finalizing code that uses rapidly evolving modules.
