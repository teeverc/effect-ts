# v3 → v4 API map: effect/SingleProducerAsyncInput

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/SingleProducerAsyncInput`

- `SingleProducerAsyncInput.AsyncInputConsumer` -> `Queue.Dequeue`: Use v4 Queue producer and consumer views and bridge to Channel with Channel.fromQueue, Channel.callback, or Channel.fromPull. Queue.make({ capacity: 0 }) is the closest rendezvous backpressure but is not one-for-one.

- `SingleProducerAsyncInput.AsyncInputProducer` -> `Queue.Enqueue`: Use v4 Queue producer and consumer views and bridge to Channel with Channel.fromQueue, Channel.callback, or Channel.fromPull. Queue.make({ capacity: 0 }) is the closest rendezvous backpressure but is not one-for-one.

- `SingleProducerAsyncInput.SingleProducerAsyncInput` -> `Queue.Queue`: Use v4 Queue producer and consumer views and bridge to Channel with Channel.fromQueue, Channel.callback, or Channel.fromPull. Queue.make({ capacity: 0 }) is the closest rendezvous backpressure but is not one-for-one.

- `SingleProducerAsyncInput.make` -> `Queue.make`: Use v4 Queue producer and consumer views and bridge to Channel with Channel.fromQueue, Channel.callback, or Channel.fromPull. Queue.make({ capacity: 0 }) is the closest rendezvous backpressure but is not one-for-one.
