# Testing Stack (@effect/vitest, Layers, Properties, Console)

Use this guide when writing tests for Effect 4.0.x code. Time control is in
`references/testing.md`. Official examples:
`references/examples/09_testing/10_effect-tests.ts` (it.effect, each, TestClock,
it.live, prop) and `references/examples/09_testing/20_layer-tests.ts`
(service test layers, shared `layer(...)`).

## Setup

- `npm i -D @effect/vitest vitest` - `@effect/vitest` 4.0.x requires
  **Vitest 5** (`vitest >=5 <6`) and the same version as `effect`.
- `@effect/vitest` re-exports everything from `vitest` (`describe`, `expect`,
  `assert`, `test`, `beforeAll`, ...) plus the Effect helpers below.
- Extra Node-style asserts: `import * as assert from "@effect/vitest/utils"`
  (`deepStrictEqual`, `strictEqual`, `assertTrue`, `assertSome`, `assertNone`,
  `assertSuccess`/`assertFailure` (Result), `assertExitSuccess`/`assertExitFailure`, `throws`, ...).

## Test helpers

| Helper | Runs with |
| --- | --- |
| `it.effect(name, () => effect)` | `Effect.scoped` + `TestClock` + `TestConsole` |
| `it.live(name, () => effect)` | `Effect.scoped`, real Clock/Console |
| `.skip`, `.only`, `.skipIf(c)`, `.runIf(c)`, `.fails`, `.each(cases)` | on both |
| `it.effect.prop` / `it.live.prop` / `it.prop` | property tests (Schema or `Arbitrary` inputs) |
| `layer(L)(name?, (it) => ...)` / `it.layer(L)` | share one built layer across a block |
| `it.flakyTest(effect, timeout?)` | retry a flaky effect until it passes or times out |
| `makeMethods(test.extend(...))` | Effect helpers over Vitest fixtures |

The test function receives Vitest's test context (`(ctx) => ...`); timeout is
the third argument (`number | TestOptions`).

```ts
import { assert, describe, it } from "@effect/vitest"
import { Effect } from "effect"

describe("basics", () => {
  it.effect("typed failures are assertable", () =>
    Effect.gen(function*() {
      const error = yield* Effect.fail("boom" as const).pipe(Effect.flip)
      assert.strictEqual(error, "boom")
    }))

  it.effect.each([[1, 2], [2, 4]] as const)("doubles %s", ([n, expected]) =>
    Effect.sync(() => assert.strictEqual(n * 2, expected)))

  it.live("real time", () => Effect.sleep("1 millis"))
})
```

A failing Effect (typed error or defect) fails the test with a pretty `Cause`.

## Services and layers

Provide test implementations per test with `Effect.provide`, or share one
instance across a block with `layer(...)`:

```ts
import { assert, describe, it, layer } from "@effect/vitest"
import { Context, Effect, Layer, Ref } from "effect"

class Users extends Context.Service<Users, {
  readonly add: (name: string) => Effect.Effect<void>
  readonly count: Effect.Effect<number>
}>()("app/Users") {
  static readonly layerTest = Layer.effect(Users, Effect.gen(function*() {
    const ref = yield* Ref.make(0)
    return Users.of({ add: () => Ref.update(ref, (n) => n + 1), count: Ref.get(ref) })
  }))
}

// Fresh instance per test
describe("per-test", () => {
  it.effect("starts empty", () =>
    Effect.gen(function*() {
      const users = yield* Users
      assert.strictEqual(yield* users.count, 0)
    }).pipe(Effect.provide(Users.layerTest)))
})

// One instance for the whole block (built once, released in afterAll).
// TestClock + TestConsole are included unless { excludeTestServices: true }.
layer(Users.layerTest)("shared", (it) => {
  it.effect("adds", () => Effect.flatMap(Users, (u) => u.add("a")))
  it.effect("sees previous test's state", () =>
    Effect.gen(function*() {
      assert.strictEqual(yield* (yield* Users).count, 1)
    }))
  it.layer(Layer.empty)("nested", (it) => {
    it.effect("still has Users", () => Effect.asVoid(Users))
  })
})
```

`layer(L, options)` options: `timeout` (layer build), `memoMap`,
`excludeTestServices`, `concurrent` (named blocks). Prefer
`Layer.provideMerge` when tests need access to a dependency of the service
under test (see `20_layer-tests.ts`).

## Property tests (native Arbitrary)

```ts
import { assert, it } from "@effect/vitest"
import { Arbitrary, Effect, Schema } from "effect"

// Inputs: array (tuple) or record of Schemas and/or Arbitraries
it.prop("addition commutes", [Schema.Finite, Schema.Int], ([a, b]) => a + b === b + a)

it.effect.prop("round-trips", { name: Schema.String, age: Schema.Int }, ({ name, age }) =>
  Effect.gen(function*() {
    assert.isString(name)
    assert.isTrue(Number.isInteger(age))
  }), { arbitrary: { runs: 200, seed: "repro-1" } })

const letters = Arbitrary.schema(Schema.Literals(["a", "b"]))
it.prop("custom arbitrary", [letters], ([l]) => l === "a" || l === "b")
```

- Returning `false`, throwing, failing, or dying falsifies and triggers
  shrinking; the failure message includes the shrunk input and a replay token.
- Options via `{ arbitrary: CheckOptions }`: `runs`, `size`, `seed`,
  `maxDiscards`, `maxShrinks`, `replay`. Global defaults:
  `Arbitrary.configureGlobal({ check: { runs: 50 } })`.
- `Arbitrary` combinators: `schema`, `Constant`, `array`, `map`, `filter`,
  `filterMap`, `flatMap`, `all`, `sampleEffect`, `checkEffect`.
- fast-check is **not** supported (no `fastCheck` option, no
  `effect/testing/FastCheck`, no `Schema.toArbitrary`).

Schema-specific checks: `new TestSchema.Asserts(schema)` from `effect/testing`
with `.decoding().succeed(input, expected)`, `.encoding().fail(input, msg)`,
`.verifyRoundTrip(options?)` (Promise) / `.verifyRoundTripEffect(options?)`.
See `references/schema.md`.

## Capturing console output

`it.effect` installs `TestConsole`, which records instead of printing:

```ts
import { assert, it } from "@effect/vitest"
import { Console, Effect } from "effect"
import { TestConsole } from "effect/testing"

it.effect("prints a greeting", () =>
  Effect.gen(function*() {
    yield* Console.log("hello", "world")
    yield* Console.error("oops")
    // arguments of all calls, flattened in order
    assert.deepStrictEqual(yield* TestConsole.logLines, ["hello", "world"])
    assert.deepStrictEqual(yield* TestConsole.errorLines, ["oops"])
  }))
// Outside it.effect: Effect.provide(TestConsole.layer)
```

To capture `Effect.log*` output instead, install a capturing `Logger.make` (see
`references/observability-examples.md`).

## Vitest fixtures

```ts
import { assert, makeMethods, test } from "@effect/vitest"
import { Effect } from "effect"

const it = makeMethods(test.extend("config", { scope: "file" }, () => ({ port: 3000 })))

it.effect("reads the fixture", ({ config }) =>
  Effect.sync(() => assert.strictEqual(config.port, 3000)))
```

Destructure fixtures in the parameter; `(ctx) =>` fails with a
`FixtureParseError` once fixtures are defined. Property tests cannot request
fixtures (auto fixtures still run).

## HTTP and RPC

- `HttpApi`: `HttpApiTest.groups(Api, ["users"])` from `effect/http-api` builds an
  in-memory typed client against your handlers (no server). Provide handlers +
  `HttpServer.layerServices`. See `references/examples/51_http-server/20_testing.ts`.
- RPC: `RpcTest.makeClient(group)` from `effect/rpc` (see
  `references/rpc-cluster-workflow.md`).
- Cluster: `TestRunner.layer` from `effect/cluster` runs entities in-memory.

## Pitfalls

- Forgetting `it.effect` provides `TestClock`: sleeps hang unless you adjust
  time (or use `it.live`).
- `layer(...)` state is shared across all tests in the block (order-dependent);
  use per-test `Effect.provide` when tests need isolation.
- Wrong Vitest major: `@effect/vitest` 4.x targets Vitest 5.

## v3 -> v4

- `it.scoped` / `it.scopedLive` -> `it.effect` / `it.live` (both are scoped).
- `TestContext`, `TestServices`, `TestLive` -> handled by `it.effect`; or
  `Layer.mergeAll(TestConsole.layer, TestClock.layer())`.
- `it.prop` uses `effect/Arbitrary` instead of fast-check;
  `TestSchema` `verifyLosslessTransformation` -> `verifyRoundTrip`.
- See `references/migration/api-map/vitest.md`.
