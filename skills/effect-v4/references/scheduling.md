# Scheduling (Schedule, Effect.repeat, Effect.schedule, Cron)

Use this guide for recurrence and time-based policies. For failure-driven retries see `references/scheduling-retry.md`. Official example: `references/examples/06_schedule/` (typechecked).

## Mental model

- `Schedule<Output, Input = unknown, Error = never, Env = never>` decides, per step, whether to continue and how long to wait. Each step sees `Schedule.Metadata` (`input`, `output`, `attempt`, `duration`, `start`, `now`, `elapsed`, `elapsedSincePrevious`; times are millisecond numbers).
- `Effect.repeat(effect, schedule)` runs the effect once, then feeds each success into the schedule. Result = the schedule's final output.
- `Effect.repeat(effect, { times | while | until | schedule })` (options form) returns the effect's own last value `A`.
- `Effect.schedule(effect, schedule)` does not add an extra initial run: `Schedule.recurs(2)` → 2 runs total (vs 3 with `repeat`). `Effect.scheduleFrom(effect, initialInput, schedule)` seeds the schedule input.
- The v4 Schedule API is small. Combinators that read "both/either/andThen" in v3 are now `max` / `min` / `concat`.

## Constructors (all verified in `effect/Schedule`)

| Constructor | Output | Behavior |
| --- | --- | --- |
| `Schedule.recurs(n)` | `number` | n recurrences, no delay. **Constructor, not a combinator.** |
| `Schedule.spaced(d)` | `number` | fixed delay after each run |
| `Schedule.fixed(d)` | `number` | fixed-rate windows (no pile-up if a run is slow) |
| `Schedule.windowed(d)` | `number` | sleep until next window boundary |
| `Schedule.exponential(base, factor = 2)` | `Duration` | base * factor^(attempt-1) |
| `Schedule.fibonacci(one)` | `Duration` | fibonacci delays |
| `Schedule.duration(d)` | `Duration` | one recurrence after `d` |
| `Schedule.during(d)` | `Duration` | recur (no delay) until `d` elapsed |
| `Schedule.cron(expr \| Cron, tz?)` | `Duration` | next cron instant; error type `Cron.CronParseError` |
| `Schedule.forever` | `number` | infinite, zero delay (replaces v3 `count`) |
| `Schedule.once` | `void` | one immediate recurrence |
| `Schedule.identity<A>()` | `A` | passes input through as output |
| `Schedule.fromStep` / `fromStepWithMetadata` | custom | build bespoke schedules (replace removed v3 `unfold`, `collect*`) |

## Combinators

- `Schedule.max([a, b, ...])` – continue while **all** continue, use the **slowest** delay (v3 `intersect`/`both`). Typical: `max([exponential, recurs(n)])` to cap attempts.
- `Schedule.min([a, b, ...])` – continue while **any** continues, use the **fastest** delay (v3 `union`/`either`). Typical: `min([exponential, spaced("10 seconds")])` to cap the delay.
- `Schedule.concat(other)` – run `self` to completion, then `other` (v3 `andThen`). `Schedule.concatResult` tags outputs: `Result.fail` for the first phase, `Result.succeed` for the second.
- `Schedule.upTo({ times?, duration? })` – stop after N recurrences and/or total elapsed time.
- `Schedule.while(meta => boolean | Effect<boolean>)` – continue while the predicate holds (replaces v3 `whileInput`, `whileOutput`, `untilInput`, `check*`). Type-guard predicates narrow input/output.
- `Schedule.jittered` – randomizes each delay to 80–120%.
- `Schedule.modifyDelay(meta => Effect<Duration.Input>)` / `Schedule.addDelay(meta => Effect<Duration.Input>)` – both effectful and receive full metadata.
- `Schedule.map(meta => B | Effect<B>)` – change output (replaces `as`, `asVoid`).
- `Schedule.tap(meta => Effect<...>)` – side effects (replaces removed `tapInput`/`tapOutput`).
- `Schedule.passthrough` – output = input.
- `Schedule.setInputType<T>()` – fix the input type for input-independent schedules before `while`/`tap` read `meta.input`.
- Low-level: `Schedule.toStep`, `toStepWithMetadata`, `toStepWithSleep` (replaces v3 `ScheduleDriver`), `Schedule.CurrentMetadata` (Context.Reference visible inside repeated/retried effects).

## Examples

```ts
import { Duration, Effect, Schedule } from "effect"

// Poll every 5s, at most 10 times. recurs(n) is a constructor; combine with max.
const poll = Schedule.max([Schedule.spaced("5 seconds"), Schedule.recurs(10)])

const healthCheck = Effect.log("ping").pipe(Effect.repeat(poll))

// Options form: returns the effect's value, not the schedule output.
const untilReady = Effect.succeed({ ready: true }).pipe(
  Effect.repeat({ until: (s) => s.ready, schedule: Schedule.spaced("1 second") })
)

// Capped exponential backoff (delay never exceeds 10s), with jitter.
export const backoff = Schedule.min([
  Schedule.exponential("250 millis"),
  Schedule.spaced("10 seconds")
]).pipe(Schedule.jittered)

// Read schedule metadata inside the repeated effect.
const report = Effect.gen(function*() {
  const meta = yield* Schedule.CurrentMetadata
  yield* Effect.log(`attempt=${meta.attempt} elapsed=${meta.elapsed}ms`)
}).pipe(Effect.repeat(Schedule.spaced("1 second").pipe(Schedule.upTo({ times: 3 }))))

// Observe without changing behavior.
const logged = Schedule.spaced("1 second").pipe(
  Schedule.tap((meta) => Effect.logDebug(`next in ${Duration.toMillis(meta.duration)}ms`))
)
```

## Cron

```ts
import { Cron, DateTime, Effect, Schedule } from "effect"

// 5 fields (minute hour day month weekday) or 6 (with leading seconds).
const weekdays9am = Schedule.cron("0 9 * * 1-5", "America/New_York")
const job = Effect.log("daily report").pipe(Effect.schedule(weekdays9am))
// Error channel includes Cron.CronParseError for string expressions.

// Calendar math without scheduling
const cron = Cron.parseUnsafe("0 0 1 * *")          // or Cron.parse -> Result<Cron, CronParseError>
const nextRun: Date = Cron.next(cron, DateTime.nowUnsafe())
const prevRun: Date = Cron.prev(cron)
const matches = Cron.match(cron, new Date())
const text = Cron.format(cron)                       // back to an expression
const upcoming = Cron.sequence(cron)                 // IterableIterator<Date>
const built = Cron.make({ minutes: [0], hours: [9], days: [], months: [], weekdays: [1, 2, 3, 4, 5] })
```

Notes: weekday `7` is Sunday (same as `0`) in both `parse` and `make`; Cron equality includes the time zone; `Cron.prev` rollover bugs were fixed during the 4.0 RC series.

## Streams

- `Stream.fromSchedule(schedule)` emits schedule outputs; `Stream.tick(d)` emits `void` every `d`.
- `Stream.fromEffectSchedule(effect, schedule)` polls an effect (see `references/examples/03_stream/10_creating-streams.ts`).
- `Stream.schedule(schedule)` spaces element emission; `Stream.repeat(schedule)` replays the whole stream; `Stream.retry(schedule)` re-runs on failure.

## v3 → v4

- Removed: `Schedule.andThen`/`andThenResult` (→ `concat`/`concatResult`), `both*`/`intersect` (→ `max`), `either*`/`union` (→ `min`), `elapsed`, `tapInput`/`tapOutput` (→ `tap`), `collectInputs`/`collectOutputs`/`collectWhile`/`delays`/`reduce`/`unfold`/`satisfies*Type` (→ `fromStep`), `count` (→ `forever`), `dayOfMonth`/`dayOfWeek`/`hourOfDay`/... (→ `cron`), `delayed` (→ `modifyDelay`), `whileInput`/`whileOutput`/`check` (→ `while`).
- `addDelay`/`modifyDelay` now receive full metadata and return an Effect.
- `Schedule.CurrentIterationMetadata` → `Schedule.CurrentMetadata`; `IterationMetadata` → `Metadata` (`attempt` instead of `recurrence`).
- Full map: `references/migration/api-map/effect-Schedule.md`, `references/migration/api-map/effect-Cron.md`.

## Pitfalls

- `Schedule.spaced(...).pipe(Schedule.recurs(3))` is wrong: `recurs` is a constructor. Use `Schedule.max([spaced, recurs(3)])` or `Schedule.upTo({ times: 3 })`.
- `Effect.schedule` does not run an extra initial attempt; `Effect.repeat` does.
- `Effect.repeat` with a schedule returns the schedule output (e.g. a count), not the effect's value; use the options form to keep `A`.
- Repetition stops at the first failure; combine with `Effect.retry` if failures should be retried.
- Use the `TestClock` (`references/testing.md`) to test schedules instead of real sleeps.
