# Streams, Queues, PubSub, STM

Use this guide for streaming and message-passing patterns.

## Mental model

- Streams emit 0..N values over time.
- Queues provide backpressure and point-to-point messaging.
- STM provides composable atomic transactions.
- Sinks consume stream values into typed aggregate results.

## Patterns

- Use `Stream.fromQueue` to turn a queue into a stream.
- Use `Queue.bounded` for backpressure.
- Use `STM.commit` to run STM transactions.
- Use `Stream.run(Sink.*)` when consumption logic should be reusable.

## Walkthrough: queue to stream

```ts
import { Effect, Queue, Stream } from "effect"

const program = Effect.gen(function*() {
  const queue = yield* Queue.bounded<number>(10)
  yield* Queue.offer(queue, 1)
  yield* Queue.offer(queue, 2)

  const stream = Stream.fromQueue(queue)
  return yield* Stream.runCollect(stream.pipe(Stream.take(2)))
})
```

## Pitfalls

- Using unbounded queues when backpressure is needed.
- Forgetting to shut down queues in long-lived apps.
- Encoding reducers directly in callbacks instead of reusable sinks.

## Docs

- `https://effect.website/docs/stream/introduction/`
- `https://effect.website/docs/stream/operations/`
- `https://effect.website/docs/concurrency/queue/`
- `https://effect.website/docs/concurrency/pubsub/`
- `https://effect.website/docs/sink/introduction/`
