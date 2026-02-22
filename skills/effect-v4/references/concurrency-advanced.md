# Concurrency Advanced (Interruption, Supervision, References) - v4

Use this guide when coordinating fibers beyond simple forking.

## Mental model

- Interruption is cooperative; attach cleanup with `Effect.onInterrupt`.
- Supervisors and scopes keep child fibers bound to a lifetime.
- In v4, fiber-local state uses `ServiceMap.Reference` — `FiberRef` has been removed.

## v4 Changes: FiberRef → ServiceMap.Reference

`FiberRef`, `FiberRefs`, `FiberRefsPatch`, and `Differ` have been removed. Fiber-local state is now handled by `ServiceMap.Reference`, which is also the mechanism for services with default values.

### Built-in Reference Mapping

| v3 FiberRef                         | v4 Reference                       |
| ----------------------------------- | ---------------------------------- |
| `FiberRef.currentConcurrency`       | `References.CurrentConcurrency`    |
| `FiberRef.currentLogLevel`          | `References.CurrentLogLevel`       |
| `FiberRef.currentMinimumLogLevel`   | `References.MinimumLogLevel`       |
| `FiberRef.currentLogAnnotations`    | `References.CurrentLogAnnotations` |
| `FiberRef.currentLogSpan`           | `References.CurrentLogSpans`       |
| `FiberRef.currentScheduler`         | `References.Scheduler`             |
| `FiberRef.currentTracerEnabled`     | `References.TracerEnabled`         |
| `FiberRef.unhandledErrorLogLevel`   | `References.UnhandledLogLevel`     |

## Reading References

References are services — `yield*` them directly:

```ts
import * as Effect from "effect/Effect"
import * as References from "effect/References"

const program = Effect.gen(function*() {
  const level = yield* References.CurrentLogLevel
  console.log(level) // "Info" (default)
})
```

## Scoped Updates (v3: `Effect.locally` → v4: `Effect.provideService`)

```ts
import * as Effect from "effect/Effect"
import * as References from "effect/References"

const program = Effect.provideService(
  myEffect,
  References.CurrentLogLevel,
  "Debug"
)
```

## Custom References

Use `ServiceMap.Reference` to define your own fiber-local state with a default:

```ts
import * as ServiceMap from "effect/ServiceMap"
import * as Effect from "effect/Effect"

const MyRef = ServiceMap.Reference<number>("MyRef", { defaultValue: () => 0 })

const program = Effect.gen(function*() {
  const value = yield* MyRef
  console.log(value) // 0
})

// Override for a region
const withOverride = Effect.provideService(program, MyRef, 42)
```

## Patterns

- Use `Effect.forkScoped` to tie a fiber to a scope.
- Use `Fiber.interrupt` and `Fiber.join` to manage lifetimes.
- Use `ServiceMap.Reference` for fiber-local state (replaces `FiberRef`).
- Use `Deferred`/`Queue`/`Semaphore` for explicit coordination.
- Use `Effect.provideService` with a reference to scope its value (replaces `Effect.locally`).

## Pitfalls

- Using removed `FiberRef` module — use `ServiceMap.Reference` instead.
- Using removed `Effect.locally` — use `Effect.provideService` instead.
- Detaching fibers without a scope.
- Assuming interruption is preemptive.

## Docs

- `https://effect.website/docs/concurrency/basic-concurrency/`
- `https://effect.website/docs/concurrency/deferred/`
- `https://effect.website/docs/concurrency/semaphore/`
- `https://effect.website/docs/observability/supervisor/`
