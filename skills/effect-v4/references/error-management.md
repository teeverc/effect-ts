# Error Management (Expected vs Defects) - v4

Use this guide when modeling failures in Effect.

## Mental model

- Expected errors are typed in the error channel and should be recoverable.
- Defects represent unexpected failures (bugs, invariants) and are not typed.
- Keep domain errors explicit and local; translate them at boundaries.

## v4 API Changes

| v3                       | v4                             |
| ------------------------ | ------------------------------ |
| `Effect.catchAll`        | `Effect.catch`                 |
| `Effect.catchAllCause`   | `Effect.catchCause`            |
| `Effect.catchAllDefect`  | `Effect.catchDefect`           |
| `Effect.catchSome`       | `Effect.catchFilter`           |
| `Effect.catchSomeCause`  | `Effect.catchCauseFilter`      |
| `Effect.catchSomeDefect` | Removed                        |
| `Effect.catchTag`        | `Effect.catchTag` (unchanged)  |
| `Effect.catchTags`       | `Effect.catchTags` (unchanged) |
| `Effect.catchIf`         | `Effect.catchIf` (unchanged)   |

## New in v4

- **`Effect.catchReason(errorTag, reasonTag, handler)`** — catches a specific `reason` within a tagged error without removing the parent error from the error channel.
- **`Effect.catchReasons(errorTag, cases)`** — like `catchReason` but handles multiple reason tags via an object of handlers.
- **`Effect.catchEager(handler)`** — an optimization variant of `catch` that evaluates synchronous recovery effects immediately.

## Patterns

- Use `Data.TaggedError` for error ADTs you plan to discriminate with `catchTag`.
- Use `Data.Error` for simple typed error classes when tags aren't needed.
- Use `Effect.catchTag` to handle specific error variants.
- Use `Effect.match` or `Effect.catch` (v4) for centralized recovery.
- Use `Effect.orDie` to convert unrecoverable failures into defects.

## Walkthrough: typed domain errors and recovery

```ts
import * as Effect from "effect/Effect"
import * as Data from "effect/Data"

class NotFound extends Data.TaggedError("NotFound")<{ readonly id: string }> {}
class Unauthorized extends Data.TaggedError("Unauthorized")<{}> {}

const fetchUser = (id: string) =>
  Effect.fail(new NotFound({ id }))

const program = fetchUser("user-1").pipe(
  Effect.catchTag("NotFound", () => Effect.succeed({ id: "guest" })),
  Effect.catchTag("Unauthorized", () => Effect.fail(new Unauthorized({})))
)
```

## Walkthrough: catch all errors (v4)

```ts
import * as Effect from "effect/Effect"

const program = Effect.fail("error").pipe(
  Effect.catch((error) => Effect.succeed(`recovered: ${error}`))
)
```

## Pitfalls

- Using `catchAll` (v3 name) instead of `catch` (v4).
- Throwing exceptions instead of returning typed failures.
- Collapsing all errors into `unknown` too early.
- Mixing expected errors and defects in the same recovery path.

## Docs

- `https://effect.website/docs/error-management/two-error-types/`
- `https://effect.website/docs/error-management/expected-errors/`
- `https://effect.website/docs/error-management/unexpected-errors/`
- `https://effect.website/docs/error-management/error-channel-operations/`
