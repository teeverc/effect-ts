---
name: effect-v4
description: "Effect v4 (beta) development and v3 → v4 migration guidance. Use when building new Effect v4 code or upgrading from v3, including API renames, behavior changes, before/after examples, and v4-specific patterns for services, layers, generators, yieldable, error handling, and schema codecs."
---

# Effect v4 (Beta) - Development & Migration

## Overview

Comprehensive guidance for Effect v4 development and migration from v3. This skill provides:

1. **v4 Core Patterns** - Guide to building new v4 code with ServiceMap, layers, generators, schema codecs
2. **v3 → v4 Migration** - Step-by-step migration guidance with API renames, behavior changes, and before/after examples

**Status:** Effect v4 is beta software under active development in [effect-smol](https://github.com/Effect-TS/effect-smol). The core programming model is stable, but `effect/unstable/*` modules may receive breaking changes in minor releases. For production use, review official v4 beta guidance and stability notes.

All bundled migration guides are sourced from the official effect-smol migration documentation.

## Quick Triage

### Building v4 Code

- Core Effect types and combinators (Result, Option, Chunk, Duration): `references/core-usage.md`
- Services and dependency injection (ServiceMap, layers): `references/dependency-management.md`
- Sequential workflows and yieldable patterns: `references/generators.md`
- Schema validation, parsing, encoding (codecs): `references/schema.md`
- Testing with TestClock and test layers: `references/testing-stack.md`

### Migrating from v3

- Runtime and run functions: `references/migration/runtime.md`
- Error handling and error channel changes: `references/migration/error-handling.md`
- Cause flattening and new structure: `references/migration/cause.md`
- Services and environment changes (Context.Tag → ServiceMap.Service): `references/migration/services.md`
- Fiber references and context locals: `references/migration/fiberref.md`
- Forking and fiber APIs (fork → forkChild, etc.): `references/migration/forking.md`
- Fiber keep-alive behavior changes: `references/migration/fiber-keep-alive.md`
- Scope and resource lifecycle patterns: `references/migration/scope.md`
- Layer memoization and Layer.fresh: `references/migration/layer-memoization.md`
- Generator and Effect.gen changes: `references/migration/generators.md`
- Yieldable protocol (non-Effect yieldables): `references/migration/yieldable.md`
- Equality and structural comparison: `references/migration/equality.md`

## Workflow

1. Ask which v3 APIs or files are in scope and which failures or regressions to avoid.
2. Open only the relevant migration notes from `references/migration/`.
3. Produce a mapping of v3 to v4 APIs, including before/after code snippets.
4. Call out behavior changes, edge cases, and test updates needed.
5. Provide a short, ordered migration checklist tailored to the code in question.

## Example Requests

- "Migrate this v3 `Runtime` usage to v4 and explain the new run functions."
- "Update our v3 error handling to v4 and show before/after examples."
- "We use FiberRefs and forking; what needs to change in v4?"
- "Explain the generator/yieldable changes and update this Effect.gen usage."
- "Do we need to change any Scope or Layer memoization behavior in v4?"

## References - v4 Core

- `references/core-usage.md` - Core Effect types and combinators
- `references/dependency-management.md` - ServiceMap, services, layers
- `references/generators.md` - Effect.gen and yieldable patterns
- `references/schema.md` - Schema codecs (decode/encode)
- `references/testing-stack.md` - TestClock and test layer composition

## References - v3 → v4 Migration

- `references/migration/cause.md` - Cause flattening and structure
- `references/migration/equality.md` - Equality and comparison changes
- `references/migration/error-handling.md` - Error channel and catch* renames
- `references/migration/fiber-keep-alive.md` - Fiber lifecycle changes
- `references/migration/fiberref.md` - FiberRef and References changes
- `references/migration/forking.md` - Fork, forkChild, forkDetach changes
- `references/migration/generators.md` - Effect.gen pattern updates
- `references/migration/layer-memoization.md` - Layer.fresh and memoization
- `references/migration/runtime.md` - Runtime and run* functions
- `references/migration/scope.md` - Scope and resource lifecycle
- `references/migration/services.md` - Context.Tag → ServiceMap.Service
- `references/migration/yieldable.md` - Yieldable protocol changes
