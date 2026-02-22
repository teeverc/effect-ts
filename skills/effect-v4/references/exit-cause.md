# Exit and Cause - v4

Use this guide when you need to inspect or report effect results.

## Mental model

- `Exit` is the result of running an effect: `Success` or `Failure`.
- A `Failure` contains a `Cause`, which captures failures, defects, and interruptions.
- In v4, `Cause<E>` is a flat wrapper around an array of `Reason` values (no more recursive tree).
- Use `Exit`/`Cause` for diagnostics or reporting where you need full result data.

## v4 Cause Structure

`Cause<E>` has been flattened from a recursive tree to a simple array of `Reason` values:

```ts
interface Cause<E> {
  readonly reasons: ReadonlyArray<Reason<E>>
}

type Reason<E> = Fail<E> | Die | Interrupt
```

The `Empty`, `Sequential`, and `Parallel` variants are gone. An empty cause is an empty `reasons` array.

## Patterns

- Use `Effect.exit` to turn failures into `Exit` values.
- Use `Exit.isFailure` / `Exit.isSuccess` to branch.
- Use `Cause.pretty` to render structured failures.
- Iterate over `cause.reasons` to handle each failure individually.

## API Changes

| v3                               | v4                               |
| -------------------------------- | -------------------------------- |
| `Cause.isFailure(cause)`         | `Cause.hasFails(cause)`          |
| `Cause.isDie(cause)`             | `Cause.hasDies(cause)`           |
| `Cause.isInterrupted(cause)`     | `Cause.hasInterrupts(cause)`     |
| `Cause.isInterruptedOnly(cause)` | `Cause.hasInterruptsOnly(cause)` |
| `Cause.failureOption(cause)`     | `Cause.findErrorOption(cause)`   |
| `Cause.dieOption(cause)`         | `Cause.findDefect(cause)`        |
| `Cause.sequential(l, r)`         | `Cause.combine(l, r)`            |
| `Cause.parallel(l, r)`           | `Cause.combine(l, r)`            |
| `Cause.isFailType(cause)`        | `Cause.isFailReason(reason)`     |
| `Cause.isDieType(cause)`         | `Cause.isDieReason(reason)`      |
| `Cause.isInterruptType(cause)`   | `Cause.isInterruptReason(reason)`|
| `Cause.NoSuchElementException`   | `Cause.NoSuchElementError`       |
| `Cause.TimeoutException`         | `Cause.TimeoutError`             |

## Walkthrough: render a failure cause

```ts
import * as Cause from "effect/Cause"
import * as Effect from "effect/Effect"
import * as Exit from "effect/Exit"

const program = Effect.fail("boom").pipe(
  Effect.exit,
  Effect.map((exit) =>
    Exit.isFailure(exit) ? Cause.pretty(exit.cause) : "ok"
  )
)
```

## Walkthrough: iterate reasons (v4)

```ts
import * as Cause from "effect/Cause"

const handle = (cause: Cause.Cause<string>) => {
  for (const reason of cause.reasons) {
    switch (reason._tag) {
      case "Fail":
        return reason.error
      case "Die":
        return reason.defect
      case "Interrupt":
        return reason.fiberId
    }
  }
}
```

## Pitfalls

- Using v3 `Cause` tree-matching patterns (switch on `Sequential`/`Parallel`) — these variants no longer exist.
- Using `Exit` when `Result` is sufficient for business logic.
- Ignoring interruptions when reporting failures.

## Docs

- `https://effect.website/docs/data-types/exit/`
- `https://effect.website/docs/data-types/cause/`
