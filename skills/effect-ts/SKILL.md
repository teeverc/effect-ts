---
name: effect-ts
description: "Effect-TS (Effect) guidance for TypeScript. Use when building, refactoring, reviewing, or explaining Effect code, especially for: typed error modeling (expected errors vs defects), Context/Layer/Effect.Service dependency wiring, Scope/resource lifecycles, runtime execution boundaries, schema-based decoding, concurrency/scheduling/streams, @effect/platform APIs, Effect AI workflows, and Promise/async migration."
---

# Effect-TS

## Overview

Provide workflows, patterns, and best practices for building Effect-based TypeScript programs, with focused references for errors, dependencies, resources, runtime execution, generators, schema, testing, platform modules, sink/stream processing, and Effect AI.

Primary docs and API sources:
- https://effect.website/llms.txt (LLM-oriented topic index)
- https://effect.website/docs
- https://effect.website/docs/platform
- https://effect.website/docs/ai/introduction
- https://effect.website/docs/additional-resources/api-reference
- https://tim-smart.github.io/effect-io-ai/ (concise API list)

For internal lookups, the effect-docs MCP can be used to search and fetch API references: https://github.com/tim-smart/effect-mcp.

## Progressive Disclosure

1. Start with `references/docs-index.md` for topic routing.
2. Read one primary guide for the task.
3. Read one adjacent guide only if needed (for cross-cutting concerns like testing, runtime, or observability).
4. Avoid loading unrelated references.

## Quick Triage

- If it needs core Effect data types or combinators, open `references/core-usage.md`.
- If it needs broader data-type choices (DateTime, BigDecimal, HashSet, Redacted), open `references/data-types-advanced.md`.
- If it needs equality/order/hash semantics, open `references/behavior-traits.md`.
- If the task is about error modeling or typed failures, open `references/error-management.md`.
- If it needs error tooling (sandboxing, Cause, error-channel transforms), open `references/error-tooling.md`.
- If it involves services/dependencies, open `references/dependency-management.md`.
- If it involves layer construction or test wiring, open `references/layer-patterns.md`.
- If it involves resource lifecycles, open `references/resource-management.md`.
- If it involves running effects or runtime choice, open `references/runtime-execution.md`.
- If it involves fibers or concurrency primitives, open `references/concurrency.md`.
- If it involves interruption, supervision, or fiber refs, open `references/concurrency-advanced.md`.
- If it involves schedules or repetition, open `references/scheduling.md`.
- If it involves retries/backoff or schedule composition, open `references/scheduling-retry.md`.
- If it involves streams, queues, pubsub, or STM, open `references/streams-queues-stm.md`.
- If it involves HTTP clients or external APIs, open `references/http-client.md`.
- If it involves HTTP servers or API definitions, open `references/http-server.md`.
- If it involves request batching or data loaders, open `references/request-resolver.md`.
- If it involves caching or memoization, open `references/caching.md`.
- If it involves configuration or config providers, open `references/configuration.md`.
- If it involves advanced config or redaction, open `references/configuration-advanced.md`.
- If it involves logs, metrics, or tracing, open `references/observability.md`.
- If it needs concrete logger/metrics/tracing setups or exporters, open `references/observability-examples.md`.
- If it needs wiring of log/metric/trace layers, open `references/observability-wiring.md`.
- If it needs sequential/branching readability, open `references/generators.md`.
- If it needs runtime validation/decoding, open `references/schema.md`.
- If it involves stream consumption patterns with reducers, open `references/sink.md`.
- If it needs deterministic time in tests, open `references/testing.md`.
- If it needs broader testing services, open `references/testing-stack.md`.
- If it involves command/file/path/terminal/key-value modules, open `references/platform-primitives.md`.
- If it involves LLM workflows, planning, or tool use via Effect AI, open `references/ai.md`.
- If it involves bundle-size constrained runtimes, open `references/micro.md`.
- If it involves migrating from Promise/async, open `references/migration-async.md`.
- If it needs versioning or signature changes, open `references/versioning.md`.
- If it hits common pitfalls or runtime errors, open `references/troubleshooting.md`.
- If it needs result inspection or debugging, open `references/exit-cause.md`.
- If it needs a docs-to-guide map from `llms.txt`, open `references/docs-index.md`.

## Core Workflow

1. Clarify boundaries and IO; keep core logic as `Effect` values.
2. Choose style: use pipelines for simple composition; use `Effect.gen` for sequential logic.
3. Model errors explicitly: type expected errors; treat defects as unexpected failures.
4. Model dependencies with services, tags, and layers; keep interfaces clean of construction concerns.
5. Manage resource lifecycles with `Scope` when opening/closing resources.
6. Provide the environment via layers and run effects only at the program edge.
7. For platform/infra code, keep side effects in dedicated adapters and expose typed services to domain code.
8. For agent tasks, include a concise rationale for error, concurrency, and runtime choices.

## Output Standards

- Show imports and minimal runnable examples.
- Prefer barrel imports (`from "effect"` / `from "@effect/platform"`) over deep module paths.
- When function-style composition is clearer, use `pipe` from `effect`.
- Keep dependency graphs explicit (services, layers, context tags).
- Include error channel types and call out expected vs defect errors.
- Avoid running effects inside libraries; show runtime usage in entrypoints or tests.
- Prefer examples that compile under current Effect major versions.

## Agent Quality Checklist

- State the target shape as `Effect<A, E, R>` when it helps design decisions.
- Explicitly separate expected errors (`E`) from defects.
- Identify where layers are provided and where `run*` is called.
- For concurrent code, state bounded/unbounded behavior and shutdown strategy.
- For boundary decoding, show Schema usage and where failures are handled.
- For tests, note which test services are provided (`TestContext`, `TestClock`, live overrides).
- For docs uncertainty, consult `references/docs-index.md` and source docs before finalizing APIs.

## References

- Start at `references/docs-index.md` to choose relevant guides quickly.
- Use `references/core-usage.md`, `references/error-management.md`, and `references/dependency-management.md` as defaults for most code tasks.
- Use platform/runtime references (`references/platform-primitives.md`, `references/runtime-execution.md`) for boundary and integration work.
- Use testing/diagnostic references (`references/testing.md`, `references/testing-stack.md`, `references/troubleshooting.md`) for verification and debugging.
