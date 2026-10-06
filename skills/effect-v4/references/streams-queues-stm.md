# Streams, Queues, PubSub, Transactions (Tx*)

Use this guide for streaming pipelines, message passing, and atomic in-memory state. Sinks: `references/sink.md`.

Official examples: `references/examples/03_stream/` (creating, consuming, NDJSON/SchemaBinary encoding) and `references/examples/01_effect/07_pubsub/10_pubsub.ts`.

## Mental model

- `Stream<A, E = never, R = never>` emits 0..N values in non-empty array batches (no `Chunk` in public APIs). Built on `Channel`.
- `Queue<A, E = never>` is point-to-point (each message to one taker). `E` is the terminal error; include `Cause.Done` in `E` when the queue can be `end`ed normally.
- `PubSub<A>` broadcasts each message to every subscriber.
- STM is `Effect.tx` + `Tx*` data types. There is no `STM` type: Tx operations return ordinary Effects.

## Creating streams

```ts
import { Effect, Queue, Schedule, Stream } from "effect"
import * as Option from "effect/Option"

Stream.make(1, 2, 3)
Stream.fromIterable([1, 2, 3])
Stream.fromArray(arr)                                   // v3 fromChunk
Stream.range(1, 100)                                    // inclusive
Stream.succeed(x) / Stream.fail(e) / Stream.empty / Stream.never
Stream.fromEffect(effect)
Stream.fromEffectSchedule(Effect.succeed(3), Schedule.spaced("30 seconds")) // polling
Stream.tick("1 second")
Stream.unfold(0, (n) => Effect.succeed(n < 5 ? [n, n + 1] as const : undefined))
Stream.paginate(0, (page) => Effect.succeed([[`item-${page}`], page < 3 ? Option.some(page + 1) : Option.none()] as const))
Stream.fromAsyncIterable(iterable, (cause) => new MyError({ cause }))
Stream.fromReadableStream({ evaluate: () => res.body!, onError: (cause) => new MyError({ cause }) })
Stream.fromEventListener<PointerEvent>(button, "click")
Stream.fromQueue(queue)            // Stream<A, Exclude<E, Cause.Done>>
Stream.fromPubSub(pubsub)
Stream.unwrap(effectOfStream)      // Effect<Stream> -> Stream (Scope excluded)
```

`Stream.callback` replaces v3 `Stream.async*`: the callback receives a `Queue` to push into; the register effect may use `Scope` for cleanup.

```ts
const clicks = Stream.callback<PointerEvent>(Effect.fn(function*(queue) {
  const onEvent = (event: PointerEvent) => Queue.offerUnsafe(queue, event)
  yield* Effect.acquireRelease(
    Effect.sync(() => button.addEventListener("click", onEvent)),
    () => Effect.sync(() => button.removeEventListener("click", onEvent))
  )
}), { bufferSize: 16, strategy: "sliding" })
// end with Queue.end(queue) / Queue.endUnsafe(queue); fail with Queue.fail(queue, e)
```

## Transforming and consuming

```ts
const pipeline = source.pipe(
  Stream.map((o) => normalize(o)),
  Stream.filter((o) => o.status === "paid"),
  Stream.mapEffect(enrich, { concurrency: 4 }),          // { unordered: true } for completion order
  Stream.flatMap((o) => Stream.fromIterable(o.lines), { concurrency: 2 }),
  Stream.tap((line) => Effect.logDebug(line)),
  Stream.take(100)
)

Stream.runCollect(pipeline)                                // Effect<Array<A>, E, R>
Stream.runDrain(pipeline)
Stream.runForEach(pipeline, (a) => Effect.log(a))
Stream.runFold(pipeline, () => 0, (acc, a) => acc + a.total) // initial is LazyArg
Stream.run(pipeline, Sink.sum)
Stream.runHead(pipeline) / Stream.runLast(pipeline)        // Option
Stream.mkString(stringStream) / Stream.mkUint8Array(byteStream)
Stream.toReadableStream(stream) / Stream.toAsyncIterable(stream)
```

Other operators (all verified): `scan`, `scanEffect`, `mapAccum`, `grouped(n)`, `groupedWithin(n, duration)`, `groupByKey(f, { bufferSize?, idleTimeToLive? })`, `sliding`, `debounce(d)`, `throttle({ cost, units, duration, burst?, strategy: "enforce" | "shape" })`, `buffer({ capacity, strategy? })`, `merge`, `mergeAll`, `concat`, `zip*`, `zipLatest`, `interleave`, `changes`, `timeout(d)`, `timeoutOrElse({ duration, orElse })`, `interruptWhen`, `haltWhen`, `aggregate`, `aggregateWithin`, `transduce`, `decodeText()`, `encodeText`, `splitLines`, `limitBytes`, `withSpan`.

Errors: `catchTag("Tag" | ["A","B"], f)`, `catchTags`, `catchIf`, `catchCause`, `catchDefect`, `catchReason(s)`, `mapError`, `orElseSucceed`, `orDie`, `retry(schedule)`, `tapError`.

Context: `Stream.provide(layer)`, `provideService`, `Stream.service(Tag)`, `Stream.serviceOption(Tag)`, `Stream.scoped`.

## Fan-out

```ts
Effect.scoped(Effect.gen(function*() {
  const [a, b] = yield* Stream.broadcastN(source, { n: 2, capacity: 16 })   // tuple of streams
  const shared = yield* Stream.share(source, { capacity: "unbounded", idleTimeToLive: "5 seconds" })
  const queue = yield* Stream.toQueue(source, { capacity: 64 })            // Dequeue<A, E | Cause.Done>
  const pubsub = yield* Stream.toPubSub(source, { capacity: 64 })
}))
// Stream.runIntoQueue(stream, queue) / Stream.runIntoPubSub(stream, pubsub, { shutdownOnEnd? })
```

## Encoding (`effect/encoding`)

```ts
import { DateTime, Schema, Stream } from "effect"
import { Ndjson, SchemaBinary } from "effect/encoding"

const decoded = textStream.pipe(Stream.pipeThroughChannel(Ndjson.decodeSchemaString(LogEntry)()))
const encoded = entries.pipe(Stream.pipeThroughChannel(Ndjson.encodeSchemaString(LogEntry)()))
// bytes: Ndjson.decode() / Ndjson.encode() / decodeSchema(S)() / encodeSchema(S)()
// options: Ndjson.decodeString({ ignoreEmptyLines: true }); errors: NdjsonError { kind: "Pack" | "Unpack" }
const frames = bytes.pipe(Stream.pipeThroughChannel(SchemaBinary.decode(MySchema)()))  // compact framed binary
```

Other codecs in `effect/encoding`: `Sse`, `Base64`, `Base64Url`, `Hex`, `Yaml`, `Toml`, `Ini`.

## Queue

```ts
import { Cause, Effect, Queue, Stream } from "effect"

const program = Effect.gen(function*() {
  const queue = yield* Queue.bounded<number, Cause.Done>(100) // also sliding(n), dropping(n), unbounded(), make({ capacity, strategy })
  yield* Queue.offer(queue, 1)                                // Effect<boolean>; suspends when full
  yield* Queue.offerAll(queue, [2, 3, 4])                     // returns the rejected values
  const one = yield* Queue.take(queue)
  const batch = yield* Queue.takeN(queue, 2)                  // waits for exactly 2 (see below)
  const upTo = yield* Queue.takeBetween(queue, 1, 10)
  const maybe = yield* Queue.poll(queue)                      // Option, never waits
  yield* Queue.end(queue)                                     // requires Cause.Done in E
  return yield* Stream.runCollect(Stream.fromQueue(queue))
})
```

- `takeN(n)` waits until **n** messages are available (changed in 4.0.0: it no longer wakes on partial batches). When the queue is ending it drains what is left; once done it fails with the terminal error (`Cause.Done` after `end`).
- `takeAll` waits for at least one message and returns all buffered ones; `collect` drains until done.
- Completion: `end` (normal), `fail(e)`, `failCause`, `interrupt`; `shutdown` discards buffered messages; `Queue.await(queue)` waits for completion. `shutdown`/`shutdownUnsafe` return `false` if already done.
- Unsafe sync variants for callbacks: `offerUnsafe`, `offerAllUnsafe`, `endUnsafe`, `failCauseUnsafe`, `shutdownUnsafe`, `takeUnsafe`, `flushUnsafe`.
- Read/write views: `Queue.Enqueue<A, E>` / `Queue.Dequeue<A, E>` (`asEnqueue`, `asDequeue`). `Queue.into(queue)(effect)` completes a queue with an effect's outcome.
- 4.0.0 breaking (custom integrations only): `Queue.State.takers` holds `Queue.Taker` entries; call `entry.resume(...)` instead of calling the entry.

v3 `Mailbox` is now `Queue` (see `references/migration/api-map/effect-Mailbox.md`, `effect-Queue.md`).

## PubSub

```ts
import { Effect, PubSub, Stream } from "effect"

const program = Effect.scoped(Effect.gen(function*() {
  const pubsub = yield* PubSub.bounded<string>({ capacity: 256, replay: 50 }) // or bounded(256), dropping, sliding, unbounded({ replay? })
  const subscription = yield* PubSub.subscribe(pubsub)                       // needs Scope
  yield* PubSub.publish(pubsub, "hello")                                     // Effect<boolean>
  yield* PubSub.publishAll(pubsub, ["a", "b"])
  const first = yield* PubSub.take(subscription)
  const rest = yield* PubSub.takeUpTo(subscription, 10)
  yield* PubSub.shutdown(pubsub)
}))
// As a stream: Stream.fromPubSub(pubsub); add `yield* Effect.addFinalizer(() => PubSub.shutdown(pubsub))` in layers.
```

Subscribers only receive messages published after they subscribe, plus up to `replay` recent ones. See `references/examples/01_effect/07_pubsub/10_pubsub.ts` for an event-bus service.

## Take, Pull, Channel (low level)

- `Pull<A, E, Done, R>` is an `Effect<A, E | Cause.Done<Done>, R>`; end of input is signalled with `Cause.done(value?)` and detected with `Cause.isDone` / `Pull.catchDone` / `Pull.isDoneCause`. `Stream.toPull(stream)` (scoped) gives `Pull<NonEmptyReadonlyArray<A>, E>`; `Stream.fromPull(effectOfPull)` builds a stream.
- `Take<A, E, Done> = NonEmptyReadonlyArray<A> | Exit<Done, E>`; `Take.toPull(take)`. Used by `Stream.toPubSubTake` / `Stream.fromPubSubTake` / `Stream.flattenTake`.
- `Channel<OutElem, OutErr, OutDone, InElem, InErr, InDone, Env>`; construct with `Channel.fromTransform((upstream, scope) => Effect<Pull>)`, compose with `Channel.pipeTo`, run with `Channel.runDrain` (`runDone` was removed). Streams interop via `Stream.fromChannel` / `Stream.toChannel` / `Stream.pipeThroughChannel`.

## Transactions (Effect.tx + Tx*)

```ts
import { Effect, TxQueue, TxRef } from "effect"

const transfer = (from: TxRef.TxRef<number>, to: TxRef.TxRef<number>, amount: number) =>
  Effect.tx(Effect.gen(function*() {
    const balance = yield* TxRef.get(from)
    if (balance < amount) return yield* Effect.txRetry   // block until a read ref changes
    yield* TxRef.update(from, (n) => n - amount)
    yield* TxRef.update(to, (n) => n + amount)
  }))

const program = Effect.gen(function*() {
  const a = yield* TxRef.make(100)
  const b = yield* TxRef.make(0)
  yield* transfer(a, b, 30)
  const q = yield* TxQueue.bounded<string>(10)
  yield* Effect.tx(TxQueue.offer(q, "job"))
  return yield* TxQueue.take(q)
})
```

- `Effect.tx(effect)` is the boundary (v3 `STM.commit`); nested `tx` calls join the outer transaction. Individual Tx ops are already atomic on their own; wrap several in one `Effect.tx` to make them atomic together.
- `Effect.txRetry` (v3 `STM.retry`) suspends until a ref read by the transaction changes.
- Modules: `TxRef`, `TxQueue`, `TxPubSub`, `TxHashMap`, `TxHashSet`, `TxChunk`, `TxDeferred`, `TxPriorityQueue`, `TxSemaphore`, `TxReentrantLock`, `TxSubscriptionRef`. All except `TxRef` export an `isTx*` guard.
- Map: `references/migration/api-map/effect-STM.md`, `effect-TRef.md`, `effect-TQueue.md`, ...

## Notable 4.0.x behavior

- `Stream.scan` emits its initial state even for empty streams (4.0.1).
- `Stream.throttle` with `strategy: "shape"` no longer drifts and uses monotonic time (4.0.1).
- `Stream.groupedWithin`, `aggregateWithin`, `aggregate` pause their schedule while upstream is idle; when the schedule ends they emit the current batch (4.0.1).
- `Stream.splitLines` treats a standalone `\r` as a terminator and flushes the final unterminated line; `decodeText` handles UTF-8 split across chunks.
- `Stream.broadcast`/`broadcastN`/`share`/`toPubSubTake` subscribers now end properly; `Stream.forever`/`repeat` no longer leak per iteration.
- Counts (`take`, `grouped`, `takeN`, ...) are normalized: fractional rounds down, `NaN`/non-positive treated as 0.

## Pitfalls

- Forgetting `Cause.Done` in a queue's error type and then calling `Queue.end` (type error) — or using `Queue.fail` for normal completion.
- `Stream.fromQueue` + never ending the queue → the stream never completes.
- Unbounded queues/pubsubs where backpressure is needed.
- Writing chunk-boundary-sensitive parsing by hand; use `splitLines`, `decodeText`, `Ndjson`, `SchemaBinary`.
- Expecting `Chunk` from `runCollect`; it returns an `Array`.
- Using v3 `STM.*` / `TRef` names; use `Effect.tx` + `TxRef`.
