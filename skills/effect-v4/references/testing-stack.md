# Testing Stack (Beyond TestClock) - v4

Use this guide when tests need more than time control.

## Mental model

- Test services are provided via layers from `effect/testing`.
- Use `TestClock.layer()` or `TestConsole.layer` when you need deterministic time or console capture.
- **v4 change:** Services are accessed via `yield*` in generators, not via static proxy methods.

## Patterns

- Use `Effect.provide` with `TestClock.layer()` to control time.
- Use `Effect.provide` with `TestConsole.layer` to capture console output.
- Use `{ local: true }` to isolate layer instances for independent test runs.

## Walkthrough: provide test console

```ts
import * as Effect from "effect/Effect"
import * as Console from "effect/Console"
import * as TestConsole from "effect/testing/TestConsole"

const program = Effect.gen(function*() {
  yield* Console.log("hello")
  const logs = yield* TestConsole.logLines
  return logs
}).pipe(Effect.provide(TestConsole.layer))

const test = Effect.runPromise(program)
```

## Practical Example: Test wiring with services (v4)

```ts
import * as Effect from "effect/Effect"
import * as Layer from "effect/Layer"
import * as ServiceMap from "effect/ServiceMap"
import * as TestClock from "effect/testing/TestClock"

interface Database {
  query: (sql: string) => Effect.Effect<unknown>
}

const Database = ServiceMap.Service<Database>("Database")

// Mock implementation for testing
const TestDatabase = Layer.succeed(Database, {
  query: () => Effect.succeed({ rows: [] })
})

// Compose test layers
const testLayers = Layer.merge(
  TestClock.layer(),
  TestDatabase
)

const program = Effect.gen(function*() {
  const db = yield* Database
  yield* db.query("SELECT *")
  return "ok"
}).pipe(Effect.provide(testLayers))

// Run test
const test = Effect.runPromise(program)
```

## Practical Example: Isolated test runs with fresh layers

```ts
import * as Effect from "effect/Effect"
import * as Layer from "effect/Layer"

const setupTest = (layer: Layer.Layer<unknown>) =>
  Effect.gen(function*() {
    // Use local: true to isolate layer instances across test runs
    yield* Effect.provide(layer, { local: true })
    return "test complete"
  })
```

## Practical Example: TestConsole and output capture

```ts
import * as Effect from "effect/Effect"
import * as Console from "effect/Console"
import * as TestConsole from "effect/testing/TestConsole"

const program = Effect.gen(function*() {
  yield* Console.log("Test message 1")
  yield* Console.log("Test message 2")
  const logs = yield* TestConsole.logLines
  return logs
}).pipe(Effect.provide(TestConsole.layer))

// Run and verify output
const test = Effect.runPromise(program).then(logs => {
  console.assert(logs.length === 2)
  console.assert(logs[0] === "Test message 1")
})
```

## Pitfalls

- Forgetting to provide the appropriate testing layer.
- Not using `{ local: true }` when you need isolated service instances per test.
- Mixing real and test implementations without explicit layer composition.
- Forgetting that in v4, services are accessed via `yield*` inside generators, not via static proxy methods.

See `references/migration/` guides for v3 → v4 testing changes.
