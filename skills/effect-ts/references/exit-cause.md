# Exit and Cause

Use this guide when you need to inspect or report effect results.

## Mental model

- `Exit` is the result of running an effect: `Success` or `Failure`.
- A `Failure` contains a `Cause`, which captures failures, defects, and interruptions.
- Use `Exit`/`Cause` for diagnostics or reporting where you need full result data.

## Patterns

- Use `Effect.exit` to turn failures into `Exit` values.
- Use `Exit.isFailure` / `Exit.isSuccess` to branch.
- Use `Cause.pretty` to render structured failures.

## Walkthrough: render a failure cause

```ts
import { Cause, Effect, Exit } from "effect"

const program = Effect.fail("boom").pipe(
  Effect.exit,
  Effect.map((exit) =>
    Exit.isFailure(exit) ? Cause.pretty(exit.cause) : "ok"
  )
)
```

## Pitfalls

- Using `Exit` when `Either` is sufficient for business logic.
- Ignoring interruptions when reporting failures.

## Docs

- `https://effect.website/docs/data-types/exit/`
- `https://effect.website/docs/data-types/cause/`
