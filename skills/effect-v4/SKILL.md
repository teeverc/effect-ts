---
name: effect-v4
description: "Migration guidance from Effect v3 to Effect v4 in TypeScript. Use when upgrading a codebase from v3 to v4, translating v3 APIs to v4 equivalents, or producing migration checklists, before/after code, and examples for runtime, services, layers, fibers, scope, generators, yieldable, and error handling changes."
---

# Effect v4 Migration

## Overview

Provide step-by-step migration guidance from Effect v3 to Effect v4. Use the bundled migration notes in `references/migration/` (copied from the official effect-smol migration docs) as the source of truth. Focus on concrete renames, behavior changes, and before/after examples.

## Quick Triage

- Runtime and run functions: `references/migration/runtime.md`
- Error handling and error channel changes: `references/migration/error-handling.md`
- Cause changes: `references/migration/cause.md`
- Services and environment changes: `references/migration/services.md`
- Fiber refs: `references/migration/fiberref.md`
- Forking and fiber APIs: `references/migration/forking.md`
- Fiber keep-alive behavior: `references/migration/fiber-keep-alive.md`
- Scope and resource lifecycle: `references/migration/scope.md`
- Layer memoization behavior: `references/migration/layer-memoization.md`
- Generators: `references/migration/generators.md`
- Yieldable protocol changes: `references/migration/yieldable.md`
- Equality changes: `references/migration/equality.md`

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

## References

- `references/migration/cause.md`
- `references/migration/equality.md`
- `references/migration/error-handling.md`
- `references/migration/fiber-keep-alive.md`
- `references/migration/fiberref.md`
- `references/migration/forking.md`
- `references/migration/generators.md`
- `references/migration/layer-memoization.md`
- `references/migration/runtime.md`
- `references/migration/scope.md`
- `references/migration/services.md`
- `references/migration/yieldable.md`
