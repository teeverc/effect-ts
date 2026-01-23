# Testing Stack (Beyond TestClock)

Use this guide when tests need more than time control.

- Use `TestContext` to provide test services (including `TestClock`).
- Provide stubs via layers and `Effect.provide`/`Layer.provide` in tests.
- Override config with `ConfigProvider.fromMap` and `Effect.withConfigProvider`.
- Prefer `Effect.runPromise` in tests to evaluate effects and assert results.
- For TestConsole/TestRandom and other test services, consult the API reference and layer them into `TestContext`.

## Example

```ts
import { Duration, Effect, TestServices } from "effect"

const program = TestServices.provideLive(
  Effect.sleep(Duration.millis(5))
)
```
