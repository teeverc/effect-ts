# Requests and Resolvers (Request / RequestResolver)

Use this guide for batching, deduplication, and caching of external lookups (data-loader pattern). Official example: `references/examples/05_batching/10_request-resolver.ts`.

## Mental model

- A `Request<Success, Error = never, Services = never>` is a value describing one lookup. Requests are compared structurally, so equal requests in the same batch are deduplicated.
- A `RequestResolver<A>` receives a non-empty array of `Request.Entry<A>` and must complete **every** entry with `entry.completeUnsafe(exit)`.
- `Effect.request(request, resolver)` (or `Effect.request(resolver)` piped) suspends until the entry is completed. Concurrent calls (e.g. `Effect.forEach(..., { concurrency })`) within the delay window are batched.
- The resolver argument may also be an `Effect<RequestResolver<A>>`.

## Defining requests

```ts
import { Request } from "effect"

// Class: type params are <Fields, Success, Error, Services>
class GetUserById extends Request.Class<{ readonly id: number }, User, UserNotFound> {}

// Tagged class (for unions / fromEffectTagged): TaggedClass(tag)<Fields, Success, Error, Services>
class GetPostById extends Request.TaggedClass("GetPostById")<{ readonly id: number }, Post, PostNotFound> {}

// Interface + constructor
interface GetTeam extends Request.Request<Team, TeamNotFound> {
  readonly _tag: "GetTeam"
  readonly id: string
}
const GetTeam = Request.tagged<GetTeam>("GetTeam")   // Request.of<R>() for untagged
```

## Defining resolvers

```ts
import { Effect, Exit, Request, RequestResolver } from "effect"

// Batched: one call for the whole batch
const UserResolver = RequestResolver.make<GetUserById>(Effect.fn(function*(entries) {
  const users = yield* fetchUsers(entries.map((e) => e.request.id)) // your batched IO
  for (const entry of entries) {
    const user = users.get(entry.request.id)
    entry.completeUnsafe(user ? Exit.succeed(user) : Exit.fail(new UserNotFound({ id: entry.request.id })))
  }
}))

// Per-request effect (each entry forked; effect receives the Entry, not the request)
const PostResolver = RequestResolver.fromEffect((entry: Request.Entry<GetPostById>) =>
  fetchPost(entry.request.id)
)

// Pure functions
RequestResolver.fromFunction((entry: Request.Entry<GetTeam>) => makeTeam(entry.request.id))
RequestResolver.fromFunctionBatched((entries: ReadonlyArray<Request.Entry<GetTeam>>) => entries.map(...)) // results in order

// Union of tagged requests: one handler per tag, each returns results in input order
type Req = GetPostById | GetTeamRequest
const Mixed = RequestResolver.fromEffectTagged<Req>()({
  GetPostById: (entries) => fetchPosts(entries.map((e) => e.request.id)),
  GetTeam: (entries) => fetchTeams(entries.map((e) => e.request.id))
})

// Group into sub-batches by a key
RequestResolver.makeGrouped<GetUserById, string>({
  key: (entry) => tenantOf(entry.request),
  resolver: (entries, tenant) => ...
})
```

`entry.context` holds the caller's context (services declared in the request's `Services` type, current span, ...).

## Tuning

```ts
const resolver = yield* RequestResolver.make<GetUserById>(runAll).pipe(
  RequestResolver.setDelay("10 millis"),          // wait for more requests (default: yieldNow)
  RequestResolver.batchN(100),                    // max batch size
  RequestResolver.withSpan("Users.resolver"),     // span + links to request spans
  RequestResolver.withCache({ capacity: 1024 })   // LRU (strategy: "lru" | "fifo"); returns an Effect
)
```

- Also: `setDelayEffect`, `grouped(f)`, `around(before, after)`, `race(that)`, `never`.
- `RequestResolver.asCache({ capacity, timeToLive? })` turns a resolver into a `Cache` (see `references/caching.md`).
- `RequestResolver.persisted({ storeId, timeToLive?, staleWhileRevalidate? })` persists results through `effect/persistence` for `Persistable.Class` requests (`@stability unstable`).

## Using inside a service (pattern from the official example)

```ts
export class Users extends Context.Service<Users, {
  getUserById(id: number): Effect.Effect<User, UserNotFound>
}>()("app/Users") {
  static readonly layer = Layer.effect(Users, Effect.gen(function*() {
    const resolver = yield* RequestResolver.make<GetUserById>(runAll).pipe(
      RequestResolver.setDelay("10 millis"),
      RequestResolver.withCache({ capacity: 1024 })
    )
    return Users.of({
      getUserById: (id) => Effect.request(new GetUserById({ id }), resolver)
    })
  }))
}

// One resolver call for unique ids [1, 2, 3]
Effect.forEach([1, 2, 1, 3, 2], (id) => users.getUserById(id), { concurrency: "unbounded" })
```

## v3 → v4

- `RequestResolver.makeBatched` → `RequestResolver.make` (entries + `completeUnsafe`); `Request.complete`/`succeed`/`fail` helpers still exist for Effect-based completion.
- Resolvers no longer carry an `R` type parameter; capture services when building the resolver (inside a Layer) or read them from `entry.context`.
- `Effect.withRequestBatching` / `withRequestCaching` and `RequestBlock` are gone; batching is on by default, caching is explicit via `withCache`/`asCache`.
- `RequestResolver.fromEffect` handlers receive a `Request.Entry` (use `entry.request`).
- Maps: `references/migration/api-map/effect-Request.md`, `effect-RequestResolver.md`, `effect-RequestBlock.md`.

## Pitfalls

- Not completing every entry: callers hang. Complete missing ids with a failure.
- Running requests sequentially (`Effect.forEach` without `concurrency`): nothing to batch.
- Forgetting `yield*` on `withCache` (it returns an Effect).
- Mixing unrelated request types in one resolver instead of `fromEffectTagged` / separate resolvers.
- Non-structural fields (functions, class instances without `Equal`) in requests defeat deduplication.
