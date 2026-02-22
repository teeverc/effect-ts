# Error Tooling (Cause and Sandboxing) - v4

Use this guide when you need to inspect or manipulate failures.

## Mental model

- The error channel contains expected errors only.
- `Cause` captures expected failures, defects, and interruptions.
- Sandboxing exposes defects as `Cause` so you can inspect everything.

## v4 API Changes

| v3                       | v4                  |
| ------------------------ | ------------------- |
| `Effect.catchAllCause`   | `Effect.catchCause` |
| `Effect.catchSomeCause`  | `Effect.catchCauseFilter` |

## Patterns

- Use `Effect.sandbox` to move defects into the error channel as `Cause`.
- Use `Effect.catchCause` (v4) to handle failures and defects together.
- Use `Cause.pretty` for diagnostics.
- Use `Effect.unsandbox` to restore defects after inspection.

## Walkthrough: inspect a defect

```ts
import * as Cause from "effect/Cause"
import * as Effect from "effect/Effect"

const program = Effect.sync(() => {
  throw new Error("boom")
}).pipe(
  Effect.sandbox,
  Effect.catchCause((cause) => Effect.succeed(Cause.pretty(cause)))
)
```

## Pitfalls

- Using `catchAllCause` (v3 name) instead of `catchCause` (v4).
- Dropping `Cause` information during recovery.
- Treating defects as recoverable business errors.

## Docs

- `https://effect.website/docs/error-management/sandboxing/`
- `https://effect.website/docs/data-types/cause/`
