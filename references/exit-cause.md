# Exit and Cause

Use this guide when you need to inspect or report effect results.

- Use `Exit` as the data type that represents the result of running an effect.
- `Exit` is either `Success` or `Failure`.
- A `Failure` carries a `Cause`, which captures the reasons for failure.
- Use `Exit`/`Cause` for diagnostics, logging, or retry/reporting logic that needs structured failure data.

## Example

```ts
import * as Effect from "effect/Effect"
import * as Exit from "effect/Exit"

const program = Effect.fail("boom").pipe(
  Effect.exit,
  Effect.map(Exit.isFailure)
)
```
