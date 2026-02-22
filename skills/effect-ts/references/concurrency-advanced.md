# Concurrency Advanced (Interruption, Supervision, FiberRef)

Use this guide when coordinating fibers beyond simple forking.

## Mental model

- Interruption is cooperative; attach cleanup with `Effect.onInterrupt`.
- Supervisors and scopes keep child fibers bound to a lifetime.
- `FiberRef` provides fiber-local state.

## Patterns

- Use `Effect.forkScoped` to tie a fiber to a scope.
- Use `Fiber.interrupt` and `Fiber.join` to manage lifetimes.
- Use `FiberRef.make` + `FiberRef.get`/`set` for context-like state.
- Use `Deferred`/`Queue`/`Semaphore` for explicit coordination instead of ad-hoc polling.

## Walkthrough: fiber-local state

```ts
import { Effect, FiberRef } from "effect"

const program = Effect.gen(function*() {
  const ref = yield* FiberRef.make(0)
  yield* FiberRef.set(ref, 1)
  return yield* FiberRef.get(ref)
})
```

## Pitfalls

- Detaching fibers without a scope.
- Assuming interruption is preemptive.

## Docs

- `https://effect.website/docs/concurrency/basic-concurrency/`
- `https://effect.website/docs/concurrency/deferred/`
- `https://effect.website/docs/concurrency/semaphore/`
- `https://effect.website/docs/observability/supervisor/`
