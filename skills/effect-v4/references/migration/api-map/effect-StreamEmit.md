# v3 → v4 API map: effect/StreamEmit

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/StreamEmit`

#### `StreamEmit.Emit`

**Replacement:** `Queue.Queue<A, E | Cause.Done>`

The StreamEmit module is gone; v4 Stream.callback hands the callback a Queue\<A, E | Cause.Done\> instead of an Emit function. Emit values with Queue.offer/Queue.offerAll, end with Queue.end, fail with Queue.fail/Queue.failCause.

**Example**

```ts
// v3: Stream.async<number, Err>((emit) => { emit.single(1); emit.end() })
Stream.callback<number, Err>((queue) =>
  Effect.gen(function*() {
    yield* Queue.offer(queue, 1)
    yield* Queue.end(queue)
  }))

```

- `StreamEmit.EmitOps` -> `Queue.offer / Queue.offerAll / Queue.end / Queue.fail / Queue.failCause`: Method-by-method mapping onto the Queue passed to Stream.callback: single(a) -\> Queue.offer(queue, a); chunk(c) -\> Queue.offerAll(queue, c); end() -\> Queue.end(queue); fail(e) -\> Queue.fail(queue, e); halt(cause) -\> Queue.failCause(queue, cause); die(d)/dieMessage(m) -\> Queue.failCause(queue, Cause.die(d)); done(exit) -\> Queue.offer then Queue.end on success, Queue.failCause on failure; fromEffect(eff) -\> run eff and offer its value (Effect.flatMap(eff, (a) =\> Queue.offer(queue, a))).

- `StreamEmit.EmitOpsPush` -> `Queue.offerUnsafe / Queue.offerAllUnsafe / Queue.endUnsafe / Queue.failCauseUnsafe`: The synchronous push interface of v3 Stream.asyncPush maps to the \*Unsafe Queue operations on the Queue given to Stream.callback: single/array -\> Queue.offerUnsafe/Queue.offerAllUnsafe, end -\> Queue.endUnsafe, fail/halt/die -\> Queue.failCauseUnsafe (wrap plain errors with Cause.fail, defects with Cause.die).
