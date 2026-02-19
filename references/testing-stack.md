# Testing Stack (Beyond TestClock)

Use this guide when tests need more than time control.

## Mental model

- Test services are provided via layers from `effect/testing`.
- Use `TestClock.layer()` or `TestConsole.layer` when you need deterministic time or console capture.

## Patterns

- Use `Effect.provide` with `TestClock.layer()` to control time.
- Use `Effect.provide` with `TestConsole.layer` to capture console output.

## Walkthrough: provide test console

```ts
import * as Effect from "effect/Effect"
import * as Console from "effect/Console"
import * as TestConsole from "effect/testing/TestConsole"

const program = Effect.gen(function*() {
  yield* Console.log("hello")
  return yield* TestConsole.logLines
}).pipe(Effect.provide(TestConsole.layer))

const test = program
```

## Pitfalls

- Forgetting to provide the appropriate testing layer.
