# Streams, Queues, PubSub, STM

Use this guide for streaming and message-passing patterns.

## Queue
- `Queue` is an in-memory structure with backpressure.
- Choose `bounded`, `sliding`, `dropping`, or `unbounded` based on capacity and overflow policy.
- Use `offer`/`take` to enqueue and dequeue values.

## PubSub
- `PubSub` broadcasts values to all subscribers rather than a single consumer.
- Use it when multiple consumers should see the same events.

## Stream
- A `Stream` is an effectful source that can emit 0..N values.
- Create with `Stream.make`, `Stream.succeed`, or `Stream.empty`.
- Consume with `runCollect`, `runDrain`, or `runForEach`.

## STM / Channel
- STM types like `TQueue` and `TPubSub` integrate with streams via `Stream.fromTQueue` and `Stream.fromTPubSub`.
- `Channel` is a lower-level stream primitive; prefer `Stream` unless you need custom chunking.
- For STM and Channel APIs, consult the Effect API reference.

## Example

```ts
import * as Effect from "effect/Effect"
import * as Queue from "effect/Queue"

const program = Effect.gen(function*() {
  const queue = yield* Queue.unbounded<number>()
  yield* Queue.offer(queue, 1)
  return yield* Queue.take(queue)
})
```
