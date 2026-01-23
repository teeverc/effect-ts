# Generators (Effect.gen)

Use this guide when sequential logic would be clearer than pipelines.

- Use `Effect.gen` to write generator-based code for sequential effects.
- `yield*` within the generator to extract values from effects in order.
- Prefer generators for multi-step workflows, branching, or early returns where pipelines become noisy.

## Example

```ts
import * as Effect from "effect/Effect"
import { pipe } from "effect/Function"

const program = pipe(
  Effect.Do,
  Effect.bind("a", () => Effect.succeed(1)),
  Effect.bind("b", ({ a }) => Effect.succeed(a + 1))
)
```
