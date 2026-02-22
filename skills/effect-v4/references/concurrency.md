# Concurrency (Fibers and Forking) - v4

Use this guide when you need concurrent execution or background work.

- Effects run on fibers, which are lightweight virtual threads managed by the Effect runtime.
- `Effect.forkChild` starts an effect in a child fiber supervised by its parent (v3: `Effect.fork`).
- `Effect.forkDetach` starts a fiber detached from parent lifecycle (v3: `Effect.forkDaemon`).
- `Effect.forkScoped` starts a child fiber tied to a local scope, independent of the parent.
- `Effect.forkIn` starts a child fiber in a specific scope for precise lifetime control.

## Mental model

- Concurrency is structured: forked work should be joined, interrupted, or scoped.
- Prefer high-level combinators (`Effect.all`, `Effect.forEach`) over manual fibers.
- Use scopes to prevent background tasks from leaking.
- In v4, the runtime keeps the process alive while fibers are suspended — `runMain` is no longer required for this, but is still recommended for signal handling.

## v4 Forking API Changes

| v3                            | v4                  |
| ----------------------------- | ------------------- |
| `Effect.fork`                 | `Effect.forkChild`  |
| `Effect.forkDaemon`           | `Effect.forkDetach` |
| `Effect.forkScoped`           | `Effect.forkScoped` (unchanged) |
| `Effect.forkIn`               | `Effect.forkIn` (unchanged) |
| `Effect.forkAll`              | Removed             |
| `Effect.forkWithErrorHandler` | Removed             |

## Fork Options (New in v4)

All fork functions accept an optional options object:

```ts
{
  readonly startImmediately?: boolean   // true: start immediately, default: deferred
  readonly uninterruptible?: boolean | "inherit"  // control interruptibility
}
```

```ts
import * as Effect from "effect/Effect"

// data-last
const fiber = myEffect.pipe(
  Effect.forkChild({ startImmediately: true })
)

// data-first
const fiber2 = Effect.forkChild(myEffect, { startImmediately: true })
```

## Walkthrough: scoped background worker

```ts
import * as Effect from "effect/Effect"
import * as Schedule from "effect/Schedule"

const worker = Effect.succeed("tick").pipe(
  Effect.repeat(Schedule.spaced("1 second"))
)

const program = Effect.scoped(
  Effect.gen(function*() {
    yield* Effect.forkScoped(worker)
    return "worker running"
  })
)
```

## Walkthrough: fork and join

```ts
import * as Effect from "effect/Effect"
import * as Fiber from "effect/Fiber"

const program = Effect.gen(function*() {
  const fiber = yield* Effect.forkChild(Effect.succeed(1))
  return yield* Fiber.join(fiber)
})
```

## Keep-Alive (New in v4)

The runtime now automatically keeps the process alive while fibers are suspended. In v3, you needed `runMain` from `@effect/platform-node` to prevent process exit while fibers waited on things like `Deferred.await`. In v4, this is built-in:

```ts
import * as Deferred from "effect/Deferred"
import * as Effect from "effect/Effect"

const program = Effect.gen(function*() {
  const deferred = yield* Deferred.make<string>()
  yield* Deferred.await(deferred) // process stays alive in v4 without runMain
})

Effect.runPromise(program)
```

`runMain` is still recommended for signal handling (`SIGINT`/`SIGTERM`) and exit code management.

## Wiring guide

- Use `Effect.all` or `Effect.forEach` with `concurrency` for bounded parallelism.
- Use `Effect.forkScoped` inside Layers or `Effect.scoped` blocks for background services.
- Prefer `Fiber.join` to observe errors; use `Fiber.interrupt` for shutdown.
- For `forkAll` replacements, use `Effect.forEach` or fork individually with `forkChild`.

## Pitfalls

- Using removed `Effect.fork` (renamed to `Effect.forkChild` in v4).
- Using removed `Effect.forkDaemon` (renamed to `Effect.forkDetach` in v4).
- Forking without join/interrupt (leaks fibers).
- Unbounded parallelism that overwhelms downstream systems.

## Docs

- `https://effect.website/docs/concurrency/basic-concurrency/`
- `https://effect.website/docs/concurrency/fibers/`
- `https://effect.website/docs/concurrency/queue/`
- `https://effect.website/docs/concurrency/pubsub/`
