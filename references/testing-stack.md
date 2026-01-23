# Testing Stack (Beyond TestClock)

Use this guide when tests need more than time control.

- Use `TestContext` to provide test services (including `TestClock`).
- Provide stubs via layers and `Effect.provide`/`Layer.provide` in tests.
- Override config with `ConfigProvider.fromMap` and `Effect.withConfigProvider`.
- Prefer `Effect.runPromise` in tests to evaluate effects and assert results.
- For TestConsole/TestRandom and other test services, consult the API reference and layer them into `TestContext`.

## Example
```ts
import * as Config from "effect/Config"
import * as ConfigProvider from "effect/ConfigProvider"
import * as Effect from "effect/Effect"
import * as TestContext from "effect/TestContext"

const program = Effect.withConfigProvider(
  Config.string("APP_NAME"),
  ConfigProvider.fromMap(new Map([["APP_NAME", "demo"]]))
)

Effect.runPromise(program.pipe(Effect.provide(TestContext.TestContext)))
```
