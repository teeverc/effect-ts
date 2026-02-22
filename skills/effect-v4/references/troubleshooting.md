# Troubleshooting (Common Errors and Fixes)

Use this guide when a task fails or produces confusing runtime behavior.

## Common issues

- Effect never runs: missing `run*` at the edge.
- Missing services: provide required layers.
- Async used with `runSync`: yields AsyncFiberException.
- Fiber leaks: fork without scope/join.
- Unexpected crashes: defects thrown instead of typed failures.
- Silent schema failures: decoding omitted at input boundaries.

## Diagnostics

- Inspect the effect type `Effect<A, E, R>`.
- Use `Effect.sandbox` + `Cause.pretty` to see defects.
- Use `Effect.tap`/`Effect.log` to observe intermediate values.
- Capture `Exit` when you need complete success/failure evidence.
- Check layer graph completeness before running the program.

## Agent debug workflow

1. Confirm the program is actually executed (`run*` at an entrypoint/test).
2. Inspect `R` requirements and verify all services are provided.
3. Determine whether failure is expected (`E`) or a defect (`Cause`).
4. For concurrency issues, verify scope ownership and interruption path.
5. For boundary inputs, add/inspect schema decode and formatter output.

## Example

```ts
import { Cause, Effect } from "effect"

const program = Effect.sync(() => {
  throw new Error("boom")
}).pipe(
  Effect.sandbox,
  Effect.catchAllCause((cause) => Effect.succeed(Cause.pretty(cause)))
)
```

## Docs

- `https://effect.website/docs/getting-started/running-effects/`
- `https://effect.website/docs/error-management/sandboxing/`
- `https://effect.website/docs/data-types/cause/`
