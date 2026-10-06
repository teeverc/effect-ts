# RPC, Cluster, Workflow and Reactivity

Use this guide for typed remote procedures (`effect/rpc`), distributed
entities (`effect/cluster`), durable workflows (`effect/workflow`) and
reactive state/atoms (`effect/reactivity` + `@effect/atom-*`). These were
separate packages in v3 (`@effect/rpc`, `@effect/cluster`, `@effect/workflow`,
`@effect/experimental`, `@effect-atom/*`); in 4.0 they live inside `effect`.
All modules here are `@stability unstable`.

Official example: `references/examples/80_cluster/10_entities.ts`.
Migration: `references/migration/api-map/rpc.md`, `cluster.md`, `workflow.md`,
`experimental.md`.

## RPC (`effect/rpc`)

### Define the contract (shared by client and server)

```ts
import { Schema } from "effect"
import { Rpc, RpcGroup } from "effect/rpc"

export class User extends Schema.Class<User>("User")({ id: Schema.String, name: Schema.String }) {}
export class UserNotFound extends Schema.TaggedError<UserNotFound>()("UserNotFound", { id: Schema.String }) {}

export const UserRpcs = RpcGroup.make(
  Rpc.make("GetUser", { payload: { id: Schema.String }, success: User, error: UserNotFound }),
  Rpc.make("ListUsers", { success: User, stream: true }),         // server stream
  Rpc.make("Ping")                                                 // void -> void
)
```

`Rpc.make(tag, { payload?, success?, error?, defect?, stream?, primaryKey? })`;
`payload` may be struct fields or a schema. Groups: `group.merge(other)`,
`group.prefix("users.")`, `group.omit("Ping")`, `group.middleware(Mw)`,
`rpc.middleware(Mw)`, `.annotate(Key, value)`.

### Implement and serve

```ts
import { NodeHttpServer, NodeRuntime } from "@effect/platform-node"
import { Effect, Layer, Stream } from "effect"
import { HttpRouter } from "effect/http"
import { RpcSerialization, RpcServer } from "effect/rpc"
import { createServer } from "node:http"

const UsersLive = UserRpcs.toLayer(Effect.gen(function*() {
  // acquire dependencies here
  return UserRpcs.of({
    GetUser: ({ id }) => id === "1" ? Effect.succeed(new User({ id, name: "Ada" })) : Effect.fail(new UserNotFound({ id })),
    ListUsers: () => Stream.make(new User({ id: "1", name: "Ada" })), // Stream or Queue
    Ping: () => Effect.void
  })
}))

const RpcRoute = RpcServer.layerHttp({ group: UserRpcs, path: "/rpc", protocol: "http" }) // default "websocket"
  .pipe(Layer.provide(UsersLive), Layer.provide(RpcSerialization.layerNdjson))

HttpRouter.serve(RpcRoute).pipe(
  Layer.provide(NodeHttpServer.layer(createServer, { port: 3000 })),
  Layer.launch,
  NodeRuntime.runMain
)
```

Lower level: `RpcServer.layer(group)` + a protocol layer
(`RpcServer.layerProtocolHttp({ path })`, `layerProtocolWebsocket({ path })`,
`layerProtocolSocketServer`, `layerProtocolStdio`, `layerProtocolWorkerRunner`).
Handler wrappers: `Rpc.fork` (run concurrently instead of sequentially),
`Rpc.uninterruptible`.

### Client

```ts
import { Context, Layer } from "effect"
import { FetchHttpClient } from "effect/http"
import { RpcClient, type RpcClientError, type RpcGroup, RpcSerialization } from "effect/rpc"

export class UsersClient extends Context.Service<
  UsersClient,
  RpcClient.RpcClient<RpcGroup.Rpcs<typeof UserRpcs>, RpcClientError.RpcClientError>
>()("app/UsersClient") {
  static readonly layer = Layer.effect(UsersClient)(RpcClient.make(UserRpcs)).pipe(
    Layer.provide(RpcClient.layerProtocolHttp({ url: "http://localhost:3000/rpc" })),
    Layer.provide([FetchHttpClient.layer, RpcSerialization.layerNdjson])
  )
}

// const client = yield* UsersClient
// const user = yield* client.GetUser({ id: "1" })   // Effect<User, UserNotFound | RpcClientError>
// const all = client.ListUsers()                    // Stream<User, RpcClientError>
```

- `RpcClient.make(group, { flatten: true })` gives `client("GetUser", payload)` style.
- WebSocket: `RpcClient.layerProtocolSocket()` + a `Socket` layer such as
  `NodeSocket.layerWebSocket("ws://.../rpc")`; workers: `RpcClient.layerProtocolWorker({ size })`.
- Headers: `RpcClient.withHeaders({...})(effect)`, `RpcClient.CurrentHeaders`.

### Serialization (must match on both sides)

| Layer | Use |
| --- | --- |
| `RpcSerialization.layerJson` | transports with framing (WebSocket, workers) |
| `RpcSerialization.layerNdjson` | HTTP / raw sockets (needs newline framing) |
| `RpcSerialization.layerJsonRpc()` / `layerNdJsonRpc()` | JSON-RPC 2.0 wire format |
| `RpcSerialization.layerSchemaBinary(options?)` | compact schema-aware binary (cluster default) |

### Middleware

```ts
import { Context, Effect, Layer, Schema } from "effect"
import { RpcMiddleware } from "effect/rpc"

class CurrentUser extends Context.Service<CurrentUser, { readonly id: string }>()("CurrentUser") {}
class Unauthorized extends Schema.TaggedError<Unauthorized>()("Unauthorized", {}) {}

class Auth extends RpcMiddleware.Service<Auth, { provides: CurrentUser }>()("Auth", {
  error: Unauthorized,
  requiredForClient: true
}) {}

const AuthLive = Layer.succeed(Auth)(Auth.of((effect, { headers }) =>
  headers.authorization ? Effect.provideService(effect, CurrentUser, { id: "u1" }) : Effect.fail(new Unauthorized())
))
// client side (requiredForClient): RpcMiddleware.layerClient(Auth, ({ next, request }) => next(request))
```

### Testing

`RpcTest.makeClient(group)` builds an in-memory client wired straight to the
handler layer (no transport/serialization):
`Layer.effect(UsersClient)(RpcTest.makeClient(UserRpcs)).pipe(Layer.provide(UsersLive))`.

## Cluster (`effect/cluster`)

Entities are RPC groups addressed by an entity id and sharded across runners.
Messages to one entity are processed sequentially by default.

```ts
import { Effect, Layer, Ref, Schema } from "effect"
import { ClusterSchema, Entity, TestRunner } from "effect/cluster"
import { Rpc } from "effect/rpc"

const Increment = Rpc.make("Increment", { payload: { amount: Schema.Int }, success: Schema.Int })
const GetCount = Rpc.make("GetCount", { success: Schema.Int })
  .annotate(ClusterSchema.Persisted, true)       // persist message in MessageStorage (default: volatile)

export const Counter = Entity.make("Counter", [Increment, GetCount])

export const CounterLive = Counter.toLayer(Effect.gen(function*() {
  const count = yield* Ref.make(0)               // in-memory state while active
  return Counter.of({
    Increment: ({ payload }) => Ref.updateAndGet(count, (n) => n + payload.amount),
    GetCount: () => Ref.get(count).pipe(Rpc.fork)
  })
}), { maxIdleTime: "5 minutes" })                // also: concurrency, mailboxCapacity, defectRetryPolicy

export const useCounter = Effect.gen(function*() {
  const clientFor = yield* Counter.client
  return yield* clientFor("counter-123").Increment({ amount: 1 })
})

export const TestLayer = CounterLive.pipe(Layer.provideMerge(TestRunner.layer)) // in-memory, single process
```

Runtimes:

- Production (Node): `NodeClusterSocket.layer({ storage?: "sql" | "local" | "byo", serialization?, runnerHealth?: "ping" | "k8s", shardingConfig?, clientOnly? })`
  from `@effect/platform-node` (default storage is SQL -> provide an
  `SqlClient`); `NodeClusterHttp.layer(...)` for HTTP transport.
- Single node with persistence: `SingleRunner.layer({ runnerStorage?: "memory" | "sql" })` (needs `SqlClient`).
- Tests/dev: `TestRunner.layer`; or `Entity.makeTestClient(Entity, layer)`.
- Config: `ShardingConfig.layer({...})`, `ShardingConfig.layerFromEnv()`.
- Other: `Singleton.make(name, effect)` (one instance cluster-wide),
  `ClusterCron.make({ name, cron, execute })`, `EntityProxy.toRpcGroup` /
  `toHttpApiGroup` to expose entities over RPC/HttpApi,
  `Entity.CurrentAddress` inside handlers.

## Workflow (`effect/workflow`)

Durable, resumable executions. Steps are `Activity`s whose results are
journaled; on replay completed activities are not re-run.

```ts
import { Effect, Schema } from "effect"
import { Activity, DurableClock, DurableDeferred, Workflow, WorkflowEngine } from "effect/workflow"

export class SendEmail extends Workflow.make("SendEmail", {   // class or const syntax
  payload: { to: Schema.String, id: Schema.String },
  success: Schema.Void,
  error: Schema.String,
  idempotencyKey: ({ id }) => id                              // derives the execution id
}) {}

export const Approval = DurableDeferred.make("Approval", { success: Schema.Boolean })

export const SendEmailLive = SendEmail.toLayer(Effect.fn(function*({ to }) {
  const draft = yield* Activity.make({
    name: "RenderEmail",
    success: Schema.String,
    execute: Effect.succeed(`Hello ${to}`)
  }).pipe(Activity.retry({ times: 3 }))
  yield* DurableClock.sleep({ name: "Cooldown", duration: "1 hour" }) // durable, survives restarts
  const approved = yield* DurableDeferred.await(Approval)          // resolved externally via token
  if (!approved) return yield* Effect.fail("rejected")
  yield* Effect.log("sending", draft)
}))

// Run (in-memory engine for dev/tests; ClusterWorkflowEngine.layer for durable cluster execution)
export const run = SendEmail.execute({ to: "a@b.c", id: "e-1" }).pipe(
  Effect.provide(SendEmailLive),
  Effect.provide(WorkflowEngine.layerMemory)
)
```

- `workflow.execute(payload, { discard: true })` returns the execution id
  without waiting; `poll(id)`, `interrupt(id)`, `resume(id)`.
- External completion: `DurableDeferred.token(d)` inside the workflow, then
  `DurableDeferred.succeed(d, { token, value })` / `fail` / `done` from outside.
- `Workflow.withCompensation((value, cause) => undo)` for saga-style rollback;
  `DurableQueue` for durable work queues; `WorkflowProxy.toRpcGroup` /
  `toHttpApiGroup` to expose workflows.
- `ClusterWorkflowEngine.layer` requires `Sharding` + `MessageStorage` (any
  cluster runtime above).

## Reactivity and Atoms (`effect/reactivity`, `@effect/atom-react`)

- `Reactivity` service: key-based invalidation.
  `Reactivity.mutation(keys)(effect)` invalidates after success,
  `Reactivity.query(keys)(effect)` / `Reactivity.stream(keys)(effect)` re-run
  when keys are invalidated, `Reactivity.invalidate(keys)`. Provide `Reactivity.layer`.
  (`effect/sql` uses it for reactive queries.)
- `Atom`: reactive state cells. `Atom.make(value | (get) => value | Effect | Stream)`,
  `Atom.fn((arg, get) => effect)` for actions, `Atom.family`, `Atom.keepAlive`,
  `Atom.swr`, `Atom.kvs`, `Atom.searchParam`, `Atom.serializable` (SSR hydration).
  Effectful atoms yield `AsyncResult` (`initial | success | failure`, with `waiting`).
- `Atom.runtime(layer)` -> `runtime.atom(effect)` / `runtime.fn(...)` for atoms that need services.
- `AtomRpc.Service<Self>()(id, { group, protocol })` and `AtomHttpApi.Service`
  turn RPC groups / HttpApi clients into `query`/`mutation` atoms with
  reactivity keys.
- React (`@effect/atom-react`, React 19): `RegistryProvider`, `useAtomValue`,
  `useAtom`, `useAtomSet`, `useAtomSuspense`, `useAtomRefresh`,
  `useAtomMount`, `HydrationBoundary`. Solid/Vue: `@effect/atom-solid`, `@effect/atom-vue`.

```tsx
import { useAtom, useAtomValue } from "@effect/atom-react"
import { Effect } from "effect"
import { AsyncResult, Atom } from "effect/reactivity"

const count = Atom.make(0)
const doubled = Atom.make((get) => get(count) * 2)
const user = Atom.make(Effect.succeed({ name: "Ada" }))

function View() {
  const [n, setN] = useAtom(count)
  const d = useAtomValue(doubled)
  const u = useAtomValue(user)
  return <button onClick={() => setN(n + 1)}>
    {d} {AsyncResult.isSuccess(u) ? u.value.name : "loading"}
  </button>
}
```

## Pitfalls

- Client and server must use the same `RpcSerialization`; HTTP needs ndjson
  framing.
- Entity handlers run one message at a time per entity; long-running reads
  need `Rpc.fork` or `concurrency`.
- Workflow code must be deterministic between activities (no direct
  `Date.now()`/random/IO outside `Activity`).
- Changing a workflow's `idempotencyKey` or activity names breaks replay of
  in-flight executions.
- v3 -> v4: `@effect/rpc` -> `effect/rpc`; `RpcServer.layerHttpRouter` ->
  `RpcServer.layerHttp`; `layerProtocol*Router` variants -> `layerProtocolHttp` /
  `layerProtocolWebsocket`; `RpcServer.toWebHandler` -> `HttpRouter.toWebHandler(RpcServer.layerHttp(...))`.
