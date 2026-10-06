# Retry Policies (Effect.retry, retryOrElse, Schedule backoff)

Use this guide when failures should be retried. Schedule constructors/combinators are listed in `references/scheduling.md`. Official example: `references/examples/06_schedule/` (production backoff, `Schedule.while` on typed errors, builder form).

## Mental model

- `Effect.retry` reruns an effect after **typed failures** (`E`). Defects and interruptions are never retried.
- The effect always runs once first; `Schedule.recurs(3)` = up to 3 retries (4 attempts).
- The schedule's **input is the error**, so `Schedule.while(({ input }) => ...)` decides retryability per error.
- When the policy stops, the **last error** is propagated (not a wrapper).

## Policy forms

```ts
Effect.retry(effect, Schedule.exponential("100 millis"))          // a Schedule
Effect.retry(effect, ($) => $(Schedule.spaced("1 second")).pipe(  // builder: infers input = E
  Schedule.while(({ input }) => input.retryable)
))
Effect.retry(effect, { times: 3 })                                  // options
Effect.retry(effect, { while: (e) => e._tag === "Timeout" })        // predicate (boolean or Effect<boolean>)
Effect.retry(effect, { until: (e) => e._tag === "Fatal", schedule: Schedule.spaced("1 second") })
Effect.retryOrElse(effect, Schedule.recurs(2), (lastError, scheduleOutput) => fallback)
```

`Retry.Options<E>` = `{ schedule?, times?, while?, until? }`. With a refinement in `while`/`until`, the error type narrows accordingly.

## Walkthrough: production backoff (adapted from examples/06_schedule)

```ts
import { Effect, Schedule, Schema } from "effect"

export class HttpError extends Schema.TaggedError<HttpError>()("HttpError", {
  message: Schema.String,
  status: Schema.Int,
  retryable: Schema.Boolean
}) {}

// 250ms exponential, delay capped at 10s, jittered, max 6 retries, retryable errors only.
export const retryPolicy = Schedule.min([
  Schedule.exponential("250 millis"),
  Schedule.spaced("10 seconds")
]).pipe(
  Schedule.jittered,
  Schedule.upTo({ times: 6 }),
  Schedule.setInputType<HttpError>(),
  Schedule.while(({ input }) => input.retryable),
  Schedule.tap(({ input, attempt }) => Effect.logWarning(`retry #${attempt} after ${input.status}`))
)

declare const fetchUser: (id: string) => Effect.Effect<{ id: string }, HttpError>

export const loadUser = Effect.fn("loadUser")(function*(id: string) {
  return yield* fetchUser(id).pipe(Effect.retry(retryPolicy))
})
```

Alternatives for capping attempts: `Schedule.max([Schedule.exponential("250 millis"), Schedule.recurs(6)])` (continue while both continue). Cap total time with `Schedule.upTo({ duration: "30 seconds" })`.

## Wrapping promises

```ts
import { Effect, Schedule, Schema } from "effect"

class FetchError extends Schema.TaggedError<FetchError>()("FetchError", { cause: Schema.Defect() }) {}

const getJson = (url: string) =>
  Effect.tryPromise({
    try: (signal) => fetch(url, { signal }).then((r) => r.json()),
    catch: (cause) => new FetchError({ cause })
  }).pipe(
    Effect.timeout("5 seconds"),                 // adds Cause.TimeoutError to E
    Effect.retry({ times: 3, schedule: Schedule.exponential("200 millis") })
  )
```

Prefer the platform `HttpClient` (`references/http-client.md`), which has `HttpClient.retryTransient` built in.

## Related tools

- Streams: `Stream.retry(schedule | builder)` re-runs the stream on failure.
- Fallback across providers/resources: `ExecutionPlan.make({ provide, attempts?, schedule?, while? }, ...)` with `Effect.withExecutionPlan` / `Stream.withExecutionPlan` (module `effect/ExecutionPlan`; e.g. try a fast model twice, then fall back to another provider layer).
- Rate limiting (not retry): `effect/persistence` `RateLimiter`, or `Stream.throttle`.

## v3 → v4

- `Effect.retry` options are the same names (`times`, `while`, `until`, `schedule`); the builder form `($) => ...` is new and replaces manual `Schedule.setInputType` in most cases.
- `Effect.retry({ times })` preserves the original error.
- Schedule combinator renames (`intersect` → `max`, `union` → `min`, `andThen` → `concat`, `whileInput` → `while`) are listed in `references/migration/api-map/effect-Schedule.md`.

## Pitfalls

- Retrying non-idempotent operations without an idempotency key.
- Unbounded policies: `Schedule.exponential` alone never stops — always add `upTo`/`recurs`/`while`.
- No jitter when many fibers or instances retry the same dependency.
- Expecting thrown exceptions (defects) to be retried; convert them to typed errors with `Effect.try`/`tryPromise` first.
- Stacking retries at several layers (client + service + caller) multiplies attempts. Retry once, at the edge.
- `Schedule.while` without an input type: use the builder form or `Schedule.setInputType<E>()` so `input` is typed.
