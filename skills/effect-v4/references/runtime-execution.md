# Running Effects (Runtime Execution) - v4

Use this guide when deciding how/where to run effects.

## Mental model

- Effects are descriptions; `run*` executes them.
- Keep `run*` calls at the edge (CLI entrypoints, server bootstrap, tests).
- Choose a runner based on sync/async and whether you need `Exit`.
- `Runtime<R>` has been removed in v4; use `ServiceMap<R>` instead.

## Patterns

- Use `Effect.runPromise` for async execution.
- Use `Effect.runSync` only for fully synchronous effects.
- Use `Effect.runFork` for background fibers.
- Use `Effect.runPromiseExit` / `Effect.runSyncExit` when you need `Exit`.
- Keep one explicit runtime boundary per app entrypoint when possible.

## v4 Changes

`Runtime<R>` no longer exists. Run functions live directly on `Effect`. The
`Runtime` module is reduced to process lifecycle utilities:

- `Runtime.Teardown` — interface for handling process exit
- `Runtime.defaultTeardown` — default teardown implementation
- `Runtime.makeRunMain` — creates platform-specific main runners

Use `ServiceMap<R>` in place of `Runtime<R>` where you previously bundled
context, flags, and fiber refs into a single value.

## Walkthrough: run and inspect Exit

```ts
import * as Effect from "effect/Effect"
import * as Exit from "effect/Exit"
import * as Cause from "effect/Cause"

const program = Effect.fail("boom")

Effect.runPromiseExit(program).then((exit) =>
  Exit.match(exit, {
    onFailure: (cause) => console.log(Cause.pretty(cause)),
    onSuccess: (value) => console.log(value)
  })
)
```

## Pitfalls

- Calling `run*` in library code (breaks composability).
- Using `runSync` on async effects.
- Dropping `Exit` when you need failure details.
- Using the removed `Runtime<R>` type — use `ServiceMap<R>` instead.

## Agent checklist

- Confirm where `run*` is called and why that boundary is correct.
- Verify `R` is fully provided before runtime execution.
- Choose `runPromiseExit` when failure diagnostics are needed by callers.
- For background fibers, ensure there is an interruption/shutdown strategy.
- Replace any `Runtime<R>` usage with `ServiceMap<R>`.
