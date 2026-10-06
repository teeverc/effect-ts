# Migrating Promise / async code to Effect (4.0.x)

Use this when wrapping Promise, callback, nullable, or AbortSignal-based APIs in
Effect, or converting `async`/`await` functions. Official example:
`references/examples/01_effect/01_basics/10_creating-effects.ts` (plus
`01_effect-gen.ts`, `02_effect-fn.ts` in the same folder).

## Mental model

- A Promise starts work immediately and runs once. An Effect is a lazy
  description; it runs when executed and can be retried, timed out, or
  interrupted.
- Effects carry typed errors (`E`) and requirements (`R`). Wrap foreign APIs once
  at the edge, then compose Effects.
- Call `Effect.runPromise` / `NodeRuntime.runMain` only at the app boundary
  (main, request handler, test). Libraries return Effects.

## Cheat sheet

| Promise world | Effect 4.0.x |
|---|---|
| `async function f(x) {...}` | `const f = Effect.fn("f")(function*(x) {...})` |
| `await p` | `yield* eff` inside `Effect.gen` / `Effect.fn` |
| `Promise` that may reject | `Effect.tryPromise({ try: (signal) => ..., catch: (e) => new MyError(...) })` |
| `Promise` that never rejects | `Effect.promise((signal) => ...)` (a rejection becomes a defect) |
| sync code that may throw | `Effect.try({ try: () => ..., catch: (e) => ... })` |
| callback / event API | `Effect.callback<A, E>((resume, signal) => { ...; return cleanupEffect })` |
| `v ?? throw` | `Effect.fromNullishOr(v)` (fails with `Cause.NoSuchElementError`) |
| `Option` value | `Effect.fromOption(opt, () => new MyError())` |
| `Promise.all(xs)` | `Effect.all(xs, { concurrency: n })` / `Effect.forEach(items, f, { concurrency: n })` |
| `Promise.allSettled` | `Effect.all(xs, { mode: "result" })` (each item becomes a `Result`) |
| `Promise.race` | `Effect.race(a, b)` / `Effect.raceAll(xs)` (losers are interrupted) |
| `setTimeout` delay | `Effect.sleep("1 second")` |
| manual timeout | `Effect.timeout("5 seconds")` (adds `Cause.TimeoutError`) |
| retry loop | `Effect.retry({ times: 3 })` / `Effect.retry(Schedule.exponential("100 millis"))` |
| `try/finally` cleanup | `Effect.ensuring`, `Effect.acquireRelease` |
| `for await (x of it)` | `Stream.fromAsyncIterable(it, (e) => new MyError(...))` |
| `AbortController` passed in | `Effect.runPromise(eff, { signal })` |

v3 → v4: `Effect.async` → `Effect.callback`; `Effect.fromNullable` →
`Effect.fromNullishOr`; `Option` values are **not** Effects any more (no
`yield* option`), so convert with `Effect.fromOption`
(`references/migration/yieldable.md`).

## Wrapping Promise APIs

The `try` thunk receives an `AbortSignal` that aborts on interruption. Pass it
to `fetch` and other cancellable APIs so `Effect.timeout` / interruption
actually cancels the work.

```ts
import { Effect, Schema } from "effect"

class UserFetchError extends Schema.TaggedError<UserFetchError>()("UserFetchError", {
  userId: Schema.String,
  cause: Schema.Defect()
}) {}

export const fetchUser = Effect.fn("fetchUser")((userId: string) =>
  Effect.tryPromise({
    try: (signal) => fetch(`/users/${userId}`, { signal }).then((r) => r.json() as Promise<{ name: string }>),
    catch: (cause) => new UserFetchError({ userId, cause })
  })
)

// Thunk-only form: failures become Cause.UnknownError
const ping = Effect.tryPromise((signal) => fetch("/health", { signal }))
```

`catch` must return the error value; if it throws, that becomes a defect.
Prefer `HttpClient` from `effect/http` over raw `fetch` for real HTTP work
(`references/http-client.md`).

## async/await → Effect.fn

```ts
import { Effect } from "effect"

// Before:
// async function loadDashboard(userId: string) {
//   const [user, team] = await Promise.all([getUser(userId), getTeam(userId)])
//   return { user, team }
// }

// fetchUser from above; fetchTeam defined the same way
export const loadDashboard = Effect.fn("loadDashboard")(function*(userId: string) {
  const [user, team] = yield* Effect.all(
    [fetchUser(userId), fetchTeam(userId)],
    { concurrency: 2 }
  )
  return { user, team }
},
  // extra combinators go as further arguments, not .pipe
  Effect.timeout("5 seconds"),
  Effect.retry({ times: 2 })
)
```

`Effect.all` defaults to sequential; set `concurrency` (number or
`"unbounded"`) explicitly. There is no inherited concurrency setting in v4.

## Callback APIs

`Effect.callback` replaces v3 `Effect.async`. Call `resume` once with an
Effect; return an Effect to run on interruption (cleanup). The second argument
is an `AbortSignal` that aborts on interruption.

```ts
import { Effect } from "effect"
import * as fs from "node:fs"

const readFileText = (path: string) =>
  Effect.callback<string, NodeJS.ErrnoException>((resume, signal) => {
    fs.readFile(path, { encoding: "utf8", signal }, (err, data) => {
      resume(err ? Effect.fail(err) : Effect.succeed(data))
    })
  })

const delayed = Effect.callback<number>((resume) => {
  const id = setTimeout(() => resume(Effect.succeed(200)), 10)
  return Effect.sync(() => clearTimeout(id)) // runs if interrupted
})
```

(For files, prefer the `FileSystem` service from `effect/FileSystem`.)

## Nullable and optional values

```ts
import { Effect, Option, Schema } from "effect"

class MissingWorkspaceId extends Schema.TaggedError<MissingWorkspaceId>()("MissingWorkspaceId", {}) {}

const headers = new Map<string, string>()

// Fails with Cause.NoSuchElementError on null/undefined; remap if needed
const workspaceId = Effect.fromNullishOr(headers.get("x-workspace-id")).pipe(
  Effect.mapError(() => new MissingWorkspaceId())
)

// Option -> Effect, with a custom error
const fromOpt = Effect.fromOption(Option.some(1), () => new MissingWorkspaceId())

// Pure (non-effectful) conversion
const opt: Option.Option<string> = Option.fromNullishOr(headers.get("x"))
```

## AbortSignal interop

- Inbound (caller cancels Effect): `Effect.runPromise(program, { signal })`
  interrupts the fiber when `signal` aborts.
- Outbound (Effect cancels a Promise API): use the `signal` given to
  `tryPromise` / `promise` / `callback`, or `Effect.abortSignal` (requires
  `Scope`; aborts when the scope closes).
- `Effect.acquireDisposable` wraps `Disposable` / `AsyncDisposable` resources.

## Running at the boundary

```ts
import { Effect, Exit, Cause } from "effect"

const value = await Effect.runPromise(program)          // rejects on failure
const exit = await Effect.runPromiseExit(program)       // never rejects
if (!Exit.isSuccess(exit)) console.error(Cause.pretty(exit.cause))
```

For long-running apps use `NodeRuntime.runMain` (`references/runtime-execution.md`,
`references/examples/01_effect/06_running/`). To call Effects from an existing
Promise-based framework, use `ManagedRuntime`
(`references/examples/04_integration/10_managed-runtime.ts`).

## Pitfalls

- `Effect.promise` for APIs that can reject: rejections become defects, not
  typed errors. Use `tryPromise`.
- Ignoring the provided `signal`: interruption/timeouts then do not stop the
  underlying work.
- Calling `Effect.runPromise` inside library code or inside another Effect
  (breaks interruption, tracing and services). Use `yield*`.
- Replacing `Promise.all` with `Effect.all` but forgetting `concurrency`
  (Effect runs sequentially by default).
- `yield*`-ing an `Option` directly (v3 habit): use `Effect.fromOption`.
- Throwing inside `Effect.gen`: throw becomes a defect. Fail with
  `return yield* new MyError(...)` instead.
