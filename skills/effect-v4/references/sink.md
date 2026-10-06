# Sink (Stream Consumers)

Use this guide when a stream should be consumed into a result by reusable logic. Streams overview: `references/streams-queues-stm.md`.

## Mental model

- `Sink<A, In = unknown, L = never, E = never, R = never>`: consumes `In` values, produces `A`, may return leftovers `L` (unconsumed input), may fail with `E`.
- Run with `Stream.run(stream, sink)` → `Effect<A, E | SinkE, R | SinkR>`.
- Many `Stream.run*` helpers are sink shortcuts (`runCollect`, `runFold`, `runForEach`, `runHead`, `runSum`, `runCount`).
- Sinks are pull-based over non-empty arrays (`Sink.transform(upstream: Pull<NonEmptyReadonlyArray<In>>, scope)`); v4 sinks no longer use `Chunk`.

## Constructors (verified in `effect/Sink`)

| Sink | Result | Leftovers |
| --- | --- | --- |
| `Sink.sum` / `Sink.count` | `number` | – |
| `Sink.collect<In>()` | `Array<In>` | – |
| `Sink.take<In>(n)` | `Array<In>` (first n) | rest |
| `Sink.head<In>()` / `Sink.last<In>()` | `Option<In>` | head: rest |
| `Sink.find(pred)` / `Sink.every(pred)` / `Sink.some(pred)` | `Option` / `boolean` | rest |
| `Sink.takeWhile(pred)` / `takeUntil` / `takeWhileEffect` | `Array<In>` | rest |
| `Sink.reduce(() => s, (s, in) => s)` | `S` | – |
| `Sink.reduceWhile(() => s, contPred, f)` / `reduceEffect` / `reduceArray` | `S` | rest |
| `Sink.fold(() => s, contPred, (s, in) => Effect<S>)` | `S` | rest |
| `Sink.foldUntil(() => s, max, f)` | `S` (after `max` inputs) | rest |
| `Sink.forEach(in => Effect)` / `forEachArray` / `forEachWhile` | `void` | – |
| `Sink.drain` / `Sink.timed` | `void` / `Duration` | – |
| `Sink.succeed(a)` / `sync` / `fail(e)` / `die` / `never` / `suspend` | – | – |
| `Sink.fromEffect(effect)` | effect result, consumes nothing | – |
| `Sink.fromQueue(queue)` | `void`; offers all input, ends queue (needs `Queue<A, Cause.Done>`) | – |
| `Sink.fromPubSub(pubsub)` | `void`; publishes all input | – |
| `Sink.fromWritableStream({ evaluate, onError, closeOnDone? })` | `void` (Web Streams) | – |
| `Sink.make<In>()(...streamPipeline)` | last effect's value; the pipeline receives the input as a `Stream<In>` | – |

Initial states are `LazyArg` thunks (`() => 0`), not values.

## Combinators

`map`, `mapEffect`, `as`, `mapInput` / `mapInputEffect` / `mapInputArray`, `mapError`, `mapLeftover`, `ignoreLeftover`, `flatMap` (sequence; the next sink receives prior leftovers), `orElse`, `catchCause`, `summarized(effect, (start, end) => x)`, `withDuration`, `provideService`, `provideContext`, `onExit`, `ensuring`, `unwrap` (Effect<Sink> → Sink).

v3 → v4: `collectAll` → `collect`, `collectAllN` → `take`, `collectAllWhile`/`Until` → `takeWhile`/`takeUntil`, `foldLeft` → `reduce`, `foldLeftChunks` → `reduceArray`, `zip*` → `flatMap`; `race*` has no replacement (run multiple consumers via `Stream.broadcastN`). See `references/migration/api-map/effect-Sink.md`.

## Examples

```ts
import { Effect, Option, Sink, Stream } from "effect"

// Reusable aggregate
const stats = Sink.reduce(
  () => ({ count: 0, total: 0 }),
  (s, n: number) => ({ count: s.count + 1, total: s.total + n })
)
const program = Stream.make(1, 2, 3).pipe(Stream.run(stats))

// Header then body: take the first element, keep the rest of the stream
const parsed = Effect.scoped(Effect.gen(function*() {
  const [header, body] = yield* Stream.make("h", "a", "b").pipe(Stream.peel(Sink.head<string>()))
  const rows = yield* Stream.runCollect(body)
  return { header: Option.getOrElse(header, () => ""), rows }
}))

// Batch stream elements with a sink (re-runs the sink until upstream ends)
const batches = Stream.range(1, 10).pipe(Stream.transduce(Sink.take<number>(3)))
// => [1,2,3], [4,5,6], [7,8,9], [10]

// Time/size-based batching uses a sink + schedule
const windowed = source.pipe(Stream.groupedWithin(100, "1 second"))

// Effectful consumer with early stop
const firstLarge = numbers.pipe(Stream.run(Sink.forEachWhile((n: number) => Effect.succeed(n < 1000))))

// Sink defined as a stream pipeline
const sumSink = Sink.make<number>()(Stream.runSum)
```

## Related stream APIs

- `Stream.run(sink)`, `Stream.peel(sink)` (scoped; returns `[result, rest]`), `Stream.transduce(sink)`, `Stream.aggregate(sink)`, `Stream.aggregateWithin(sink, schedule)`, `Stream.tapSink(sink)`.

## Pitfalls

- Passing a value where a `LazyArg` is expected (`Sink.reduce(0, f)` is a type error; use `() => 0`).
- Ignoring leftovers when sequencing sinks over framed protocols; `flatMap` forwards leftovers, `ignoreLeftover` drops them.
- `Stream.peel` requires a `Scope` (wrap in `Effect.scoped`).
- Treating expected parse failures as defects: return typed errors from `mapEffect`/`fold` instead of throwing.
- Expensive sinks on unbounded fast streams without `buffer`/`throttle`.
