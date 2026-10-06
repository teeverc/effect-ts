# v3 → v4 API map: effect/Stream

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Stream`

- `Stream.Stream` -> `Stream`: The Stream\<A, E, R\> interface is unchanged in shape and keeps the effect/Stream import path; the type-id key is now the string literal "\~effect/Stream" instead of a unique symbol.

- `Stream.Stream.Context` -> `Stream.Services`: Type-level extractor of the R parameter renamed from Stream.Context\<T\> to Stream.Services\<T\>; identical conditional-infer semantics.

- `Stream.Stream.DynamicTuple` -> `Types.TupleOf`: Already deprecated in v3 in favor of Types.TupleOf; removed in v4. Use Types.TupleOf\<N, T\> (v4 Stream.broadcastN uses it for its return type).

- `Stream.Stream.DynamicTupleOf` -> `Types.TupleOf`: Recursive tuple-builder helper removed with Stream.DynamicTuple; Types.TupleOf\<N, T\> is the v4 equivalent.

- `Stream.StreamTypeId` -> `Stream.TypeId`: Renamed StreamTypeId -\> TypeId and changed from a unique symbol to the string literal "\~effect/Stream" (both the const and the type).

- `Stream.StreamUnify` -> `Stream.StreamUnify`: Still exported under the same name in v4 (extends Effect.EffectUnify); no change needed besides any Chunk-related element types.

- `Stream.StreamUnifyIgnore` -> `Stream.StreamUnifyIgnore`: Still exported under the same name in v4 (extends Effect.EffectUnifyIgnore with Effect ignored); unchanged.

- `Stream.accumulateChunks` -> `none`: v3 accumulateChunks only rewrote the internal chunk layout (each chunk cumulatively contained all prior elements) without changing the emitted element sequence; v4 has no chunk-layout twin. Stream.accumulate emits the cumulative NonEmptyArray values, and Stream.rechunk controls chunk sizing.

- `Stream.acquireRelease` -> `Stream.scoped(Stream.fromEffect(Effect.acquireRelease(acquire, release)))`: Dedicated constructor removed; compose Effect.acquireRelease (same (resource, exit) release signature) with Stream.fromEffect, then Stream.scoped to tie the finalizer to the stream's lifetime.

- `Stream.aggregateWithinEither` -> `Stream.aggregateWithin`: Either-emitting variant removed; v4 aggregateWithin(sink, schedule) emits only the sink outputs B (schedule outputs are no longer surfaced as Either.right).

- `Stream.as` -> `Stream.as`: Retained in v4 with the same signature; replaces every element with the provided constant value.

- `Stream.async` -> `Stream.callback`: Stream.callback((queue) =\> Effect | void, { bufferSize?, strategy? }) replaces the Emit-based async; push with Queue.offer/offerAll, end with Queue.end, fail with Queue.fail.

- `Stream.asyncEffect` -> `Stream.callback`: The register function of Stream.callback may return an Effect (run before the stream starts pulling), covering asyncEffect; signal end/failure through the provided Queue.

- `Stream.asyncPush` -> `Stream.callback`: Stream.callback's register effect can use Scope for acquire/release of the external subscription, replacing asyncPush; the Emit ops helpers become plain Queue operations.

- `Stream.asyncScoped` -> `Stream.callback`: Stream.callback's register effect may use Scope (Scope is excluded from the resulting R), replacing asyncScoped; the Option\<E\> end signal becomes Queue.end.

- `Stream.bind` -> `Stream.bind`: The input must be a record. Start from Stream.Do or use Stream.bindTo(name) to wrap a non-record element. The field name may now re-bind an existing key; the resulting record type is Simplify\<Omit\<A, N\> & Record\<N, B\>\>.

- `Stream.bindEffect` -> `Stream.bindEffect`: The input must be a record. Start from Stream.Do or use Stream.bindTo(name) to wrap a non-record element. Existing keys may be re-bound, with the new value replacing the old field type. The callback still returns an Effect.

#### `Stream.branchAfter`

**Replacement:** `Stream.peel`

Removed; the closest v4 primitive is Stream.peel(self, Sink.take(n)), a scoped Effect yielding [firstN, restStream] from which you build the continuation stream and re-wrap with Stream.unwrap.

**Example**

```ts
Stream.unwrap(Effect.map(Stream.peel(self, Sink.take(n)), ([head, rest]) => f(head)(rest)))
```

- `Stream.broadcastDynamic` -> `Stream.broadcast`: v4 Stream.broadcast({ capacity, strategy?, replay? }) is the dynamic-subscriber fan-out returning Effect\<Stream\<A, E\>, never, Scope | R\> (v3 fixed-arity broadcast(n) became Stream.broadcastN); Stream.share adds refcounted/idleTimeToLive semantics.

- `Stream.broadcastedQueues` -> `none`: Queue-of-Take fan-out surface removed. Use Stream.broadcastN({ n, capacity }) for a fixed tuple of mirror streams, or Stream.toPubSubTake to obtain a PubSub of Take values and subscribe consumers to it.

- `Stream.broadcastedQueuesDynamic` -> `none`: Removed with broadcastedQueues. Use Stream.broadcast (dynamic mirror streams) or Stream.toPubSubTake + PubSub subscriptions when raw Take-level consumers are needed.

- `Stream.bufferChunks` -> `Stream.bufferArray`: Chunk-\>Array rename; buffers whole arrays (chunks) up to capacity with the same strategy options.

- `Stream.catchAll` -> `Stream.catch`: Renamed to Stream.catch (exported keyword-style); same (error) =\> Stream handler for all typed failures.

- `Stream.catchAllCause` -> `Stream.catchCause`: Renamed; handler receives the full Cause\<E\> and returns a recovery stream, identical semantics.

- `Stream.catchSome` -> `Stream.catchFilter`: Option-returning partial handler replaced by the Filter API: Stream.catchFilter(filter, f, orElse?) recovers matched errors, unmatched failures pass through (Stream.catchIf for refinement/predicate matching).

- `Stream.catchSomeCause` -> `Stream.catchCauseFilter`: Option-returning cause handler replaced by Stream.catchCauseFilter(filter, f, orElse?) using a Filter on the Cause (Stream.catchCauseIf for refinements).

- `Stream.catchTags` -> `Stream.catchTags`: Only keys present in the tagged error union are accepted; remove handlers for tags the stream cannot fail with. Untagged errors remain in the error channel.

- `Stream.chunksWith` -> `Stream.flattenArray(f(Stream.chunks(self)))`: No dedicated combinator; expose chunk structure with Stream.chunks (Stream\<NonEmptyReadonlyArray\<A\>\>), transform, then re-flatten with Stream.flattenArray.

- `Stream.combineChunks` -> `Stream.combineArray`: Chunk-\>Array rename of the pull-level combining primitive; pulls now yield NonEmptyReadonlyArray values and halt via Cause.Done-failing Pull effects instead of Option-typed errors.

- `Stream.concatAll` -> `Stream.flatten`: Chunk-of-streams constructor removed; sequential concatenation of many streams is Stream.flatten(Stream.fromIterable(streams)) (default concurrency 1 preserves order).

- `Stream.context` -> `Stream.fromEffect(Effect.context<R>())`: Dedicated accessor removed; lift Effect.context\<R\>() into a single-element stream.

- `Stream.contextWith` -> `Stream.fromEffect(Effect.contextWith(f))`: Dedicated accessor removed; Effect.contextWith still exists in v4, lift it with Stream.fromEffect.

- `Stream.contextWithEffect` -> `Stream.fromEffect(Effect.flatMap(Effect.context<R0>(), f))`: Removed; read the Context with Effect.context, feed it to the effectful function, and lift the result with Stream.fromEffect.

- `Stream.contextWithStream` -> `Stream.unwrap(Effect.contextWith(f))`: Removed; build the dependent stream inside Effect.contextWith and flatten with Stream.unwrap.

- `Stream.crossLeft` -> `Stream.crossWith(that, (a, _) => a)`: Removed; cartesian product keeping only left elements is expressed with Stream.crossWith and a left-projecting combiner.

- `Stream.crossRight` -> `Stream.crossWith(that, (_, b) => b)`: Removed; cartesian product keeping only right elements is Stream.crossWith with a right-projecting combiner (equivalently Stream.flatMap(self, () =\> that)).

- `Stream.dieMessage` -> `Stream.die(new Error(message))`: Removed along with RuntimeException; die with an explicit defect value via Stream.die.

- `Stream.dieSync` -> `Stream.failCauseSync(() => Cause.die(evaluate()))`: Removed; lazily construct the defect cause with Cause.die inside Stream.failCauseSync.

- `Stream.distributedWith` -> `none`: Predicate-routed fixed fan-out to Take queues removed (no v4 counterpart found among broadcast/broadcastN/share/toPubSub/partition). Closest patterns: Stream.broadcastN + Stream.filter per branch, Stream.partition for two-way splits, or manual routing by running the stream into per-consumer Queues.

- `Stream.distributedWithDynamic` -> `none`: Dynamic predicate-routed fan-out removed with distributedWith. Use Stream.broadcast/Stream.share for dynamic mirrors plus per-subscriber Stream.filter, or hand-roll routing into Queues via Stream.runForEach.

- `Stream.either` -> `Stream.result`: Either is replaced by Result in v4: Stream.result yields Stream\<Result.Result\<A, E\>, never, R\> (element -\> Result.succeed, first error -\> Result.fail and the stream ends, as before).

- `Stream.ensuringWith` -> `Stream.onExit`: Renamed; Stream.onExit runs the finalizer with the Exit\<unknown, E\> of the stream, identical shape.

- `Stream.execute` -> `Stream.fromEffectDrain`: Renamed; runs the effect for its side effects and emits nothing (Stream\<never, E, R\>).

- `Stream.filterMapWhile` -> `Stream.takeWhileFilter`: Option-returning partial function replaced by the Filter API: Stream.takeWhileFilter(filter) maps and emits while the filter passes, ending the stream at the first miss.

- `Stream.filterMapWhileEffect` -> `none`: No effectful takeWhileFilter variant in v4. Recreate by using Stream.takeWhileFilter with a Filter that selects the Effect\<A2, E2, R2\> to run, followed by Stream.mapEffect((eff) =\> eff) to execute it.

- `Stream.finalizer` -> `Stream.ensuring`: One-element finalizer-registering stream removed; attach finalizers directly with Stream.ensuring/Stream.onExit, or register in the stream scope via Stream.scoped(Stream.fromEffect(Effect.addFinalizer(fin))) when the v3 concat-a-finalizer pattern must be preserved.

- `Stream.find` -> `Stream.take(Stream.filter(self, predicate), 1)`: Removed; first-match semantics are Stream.filter followed by Stream.take(1).

- `Stream.findEffect` -> `Stream.take(Stream.filterEffect(self, f), 1)`: Removed; Stream.filterEffect takes an effectful (a, index) =\> Effect\<boolean\> predicate, then Stream.take(1) stops at the first match.

- `Stream.flattenChunks` -> `Stream.flattenArray`: Chunk-\>Array rename; flattens a Stream of ReadonlyArray values into their elements.

- `Stream.flattenExitOption` -> `Stream.flattenTake`: The Exit\<A, Option\<E\>\> end-of-stream encoding is gone; v4 uses Take\<A, E, Done\> = NonEmptyReadonlyArray\<A\> | Exit and Stream.flattenTake unwraps it (emit arrays, end/fail on Exit).

- `Stream.flattenIterables` -> `Stream.flattenIterable`: Renamed (singular); flattens a Stream of Iterables into their elements.

- `Stream.fromChunk` -> `Stream.fromArray`: Chunk-\>Array rename; takes a ReadonlyArray and emits it as one chunk.

- `Stream.fromChunkPubSub` -> `Stream.fromPubSub`: Chunked PubSub constructors are gone; v4 Stream.fromPubSub(pubsub) consumes PubSub\<A\> directly (batched internally). For a PubSub carrying arrays use Stream.flattenArray(Stream.fromPubSub(pubsub)); the scoped/shutdown options were dropped (Stream.fromSubscription consumes an existing subscription).

- `Stream.fromChunkQueue` -> `Stream.fromQueue`: Chunked Queue constructor gone; v4 Stream.fromQueue consumes Queue.Dequeue\<A, E\> whose done/failure signals end the stream (no shutdown option). For array payloads wrap with Stream.flattenArray.

- `Stream.fromChunks` -> `Stream.fromArrays`: Chunk-\>Array rename; variadic arrays, each emitted as one chunk.

- `Stream.fromEffectOption` -> `none`: The Effect\<A, Option\<E\>\> encoding (fail None = empty stream) is removed; v4 signals early end with Cause.Done in Pull-level code. Rebuild with Stream.unwrap: map the success to Stream.succeed and match the Option error to Stream.empty (None) or Stream.fail (Some).

- `Stream.fromReadableStreamByob` -> `Stream.fromReadableStream`: BYOB reader variant removed (no byob support in v4 source); Stream.fromReadableStream({ evaluate, onError, releaseLockOnEnd? }) consumes any ReadableStream with a default reader, without byte-buffer allocation control.

- `Stream.fromTPubSub` -> `none`: STM TPubSub was replaced by the transactional TxPubSub module and v4 Stream has no Tx\* constructors; subscribe and repeatedly TxQueue.take from the subscription (e.g. inside Stream.fromPull/Stream.callback), or bridge through a regular PubSub and Stream.fromPubSub.

- `Stream.fromTQueue` -> `none`: STM TQueue was replaced by TxQueue and v4 Stream has no Tx\* constructors; drain by repeatedly calling TxQueue.take inside a custom loop (Stream.fromPull/Stream.callback), or bridge into a regular Queue and use Stream.fromQueue.

- `Stream.haltAfter` -> `Stream.haltWhen(Effect.sleep(duration))`: Duration-specialized halt removed; v3 documented it as haltWhen with a sleep — completes the stream after the duration without interrupting an in-flight pull.

- `Stream.haltWhenDeferred` -> `Stream.haltWhen(Deferred.await(deferred))`: Deferred-specialized variant removed; Deferred.await is an Effect, so plain Stream.haltWhen covers it.

- `Stream.identity` -> `Channel.identity`: The identity-pipeline Stream is gone; for pipeThrough-style plumbing use Stream.pipeThroughChannel(Channel.identity()), or simply the identity function where a Stream=\>Stream transform is expected.

- `Stream.interruptAfter` -> `Stream.interruptWhen(Effect.sleep(duration))`: Duration-specialized interrupt removed; interruptWhen forks the sleep and also interrupts an in-progress pull, matching v3 semantics.

- `Stream.interruptWhenDeferred` -> `Stream.interruptWhen(Deferred.await(deferred))`: Deferred-specialized variant removed; pass Deferred.await to Stream.interruptWhen (a Deferred failure surfaces as the stream's failure, as before).

- `Stream.let` -> `Stream.let`: Retained in v4 and aligned with Effect.let: the field name may re-bind an existing key (the computed value replaces it) and the record type is Simplify\<Omit\<A, N\> & Record\<N, B\>\>.

- `Stream.mapBoth` -> `Stream.mapBoth`: Rename the options onSuccess to onElement and onFailure to onError. The callbacks still transform emitted elements and typed errors respectively.

- `Stream.mapChunks` -> `Stream.mapArray`: Chunk-\>Array rename; transforms each emitted chunk as a NonEmptyReadonlyArray.

- `Stream.mapChunksEffect` -> `Stream.mapArrayEffect`: Chunk-\>Array rename of the effectful per-chunk transform.

- `Stream.mapConcat` -> `Stream.flattenIterable(Stream.map(self, f))`: Removed; map each element to an Iterable and flatten with Stream.flattenIterable.

- `Stream.mapConcatChunk` -> `Stream.flattenArray(Stream.map(self, f))`: Chunk variant removed with Chunk itself; map to a ReadonlyArray and flatten with Stream.flattenArray.

- `Stream.mapConcatChunkEffect` -> `Stream.flattenArray(Stream.mapEffect(self, f))`: Removed; effectfully map each element to a ReadonlyArray and flatten with Stream.flattenArray.

- `Stream.mapConcatEffect` -> `Stream.flattenIterable(Stream.mapEffect(self, f))`: Removed; effectfully map each element to an Iterable and flatten with Stream.flattenIterable.

- `Stream.mapErrorCause` -> `Stream.catchCause((cause) => Stream.failCause(f(cause)))`: Removed; transform the full Cause by catching it and re-failing with the mapped cause.

- `Stream.mapInputContext` -> `Stream.updateContext`: Renamed; same contravariant (Context\<R2\>) =\> Context\<R\> mapping of the required services.

- `Stream.mergeEither` -> `Stream.mergeResult`: Either replaced by Result: Stream.mergeResult(self, that) yields Result.Result\<A, A2\> with self -\> Result.succeed and that -\> Result.fail (v3 put self in Either.left and that in Either.right, so the success/left roles swap sides).

- `Stream.mergeWith` -> `Stream.merge(Stream.map(self, onSelf), Stream.map(that, onOther), { haltStrategy })`: Removed; pre-map both streams to the common type and use Stream.merge, whose options accept the same haltStrategy union ("left" | "right" | "both" | "either").

#### `Stream.mergeWithTag`

**Replacement:** `none`

Struct-to-tagged-union merge removed. Recreate with Stream.mergeAll over the entries, tagging each stream first.

**Example**

```ts
Stream.mergeAll(Object.entries(streams).map(([_tag, s]) => Stream.map(s, (value) => ({ _tag, value }))), { concurrency })
```

- `Stream.onDone` -> `Stream.onEnd`: Renamed; v4 onEnd takes an Effect value (not a () =\> Effect thunk) run when the stream ends successfully, and its error type may add to the stream's.

- `Stream.orDieWith` -> `Stream.orDie`: orDieWith removed; transform the error first, then convert failures to defects: `self.pipe(Stream.mapError(f), Stream.orDie)`.

- `Stream.orElse` -> `Stream.catch`: v3 catchAll was renamed to Stream.catch in v4; orElse ignored the error, so write `Stream.catch(self, () => that())`.

- `Stream.orElseEither` -> `Stream.catch`: Removed; Either is replaced by Result in v4. Emulate: `Stream.map(self, Result.succeed).pipe(Stream.catch(() => Stream.map(that(), Result.fail)))` (same encoding v4 Stream.mergeResult uses).

- `Stream.orElseFail` -> `Stream.mapError`: Removed; it only replaced the failure value: `Stream.mapError(self, () => error())` or `Stream.catch(self, () => Stream.fail(error()))`.

- `Stream.orElseIfEmptyChunk` -> `Stream.orElseIfEmpty`: Folded into Stream.orElseIfEmpty, which now takes a lazy fallback Stream: `Stream.orElseIfEmpty(self, () => Stream.fromArray(array))`; Chunk is replaced by plain arrays.

- `Stream.orElseIfEmptyStream` -> `Stream.orElseIfEmpty`: Direct rename: v4 Stream.orElseIfEmpty takes a LazyArg\<Stream\> fallback, identical semantics.

- `Stream.paginateChunk` -> `Stream.paginate`: v4 Stream.paginate is effectful and array-based: `paginate(s, (s) => Effect<readonly [ReadonlyArray<A>, Option<S>]>)`; wrap the pure step in Effect.succeed and use an array instead of a Chunk.

- `Stream.paginateChunkEffect` -> `Stream.paginate`: v4 Stream.paginate has exactly this shape; only Chunk becomes ReadonlyArray.

- `Stream.paginateEffect` -> `Stream.paginate`: v4 Stream.paginate emits a batch per step; wrap the single value in an array: `(s) => Effect.map(step(s), ([a, next]) => [[a], next])`.

- `Stream.partition` -> `Stream.partition`: Pass a Result-returning Filter instead of a boolean predicate or refinement; Filter.fromPredicate(predicate) adapts existing predicates. The tuple order is now [passes, fails], the reverse of v3 [excluded, satisfying]. Rename bufferSize to capacity (default 16; also accepts "unbounded"). Acquisition is scoped and does not fail with E; the returned streams carry E.

- `Stream.partitionEither` -> `Stream.partitionEffect`: Either-based split replaced by Filter.FilterEffect: the function now returns Effect\<Result\<Pass, Fail\>\> (Result.succeed/Result.fail instead of Either.right/left). Returns Effect\<[passes, fails], never, R | Scope\> — note the tuple is [passes, fails], v3 was [left, right]; options are { capacity?, concurrency? }.

- `Stream.provideLayer` -> `Stream.provide`: v4 Stream.provide accepts a Layer or a Context; behavior identical.

- `Stream.provideServiceStream` -> `none`: Removed; v4 has provideService/provideServiceEffect but no stream-valued variant. Emulate with `Stream.flatMap(services, (s) => Stream.provideService(self, tag, s))` over the service stream, or use Stream.provideServiceEffect for effectful acquisition.

- `Stream.provideSomeContext` -> `Stream.provideContext`: v4 Stream.provideContext is the single Context provider with `Exclude<R, R2>` semantics — same behavior as v3 provideSomeContext.

- `Stream.provideSomeLayer` -> `Stream.provide`: v4 Stream.provide accepts a Layer (or Context) and excludes only the provided services from R — same partial-provision semantics.

- `Stream.refineOrDie` -> `Stream.catch`: Removed; emulate with `Stream.catch(self, (e) => { const r = pf(e); return Option.isSome(r) ? Stream.fail(r.value) : Stream.die(e) })` — refail refined errors, die on the rest.

- `Stream.refineOrDieWith` -> `Stream.catch`: Removed; same as refineOrDie but die with the mapped defect: `Stream.die(f(e))` for unrefined errors.

- `Stream.repeatEffect` -> `Stream.fromEffectRepeat`: Renamed; repeats the effect forever emitting each result.

- `Stream.repeatEffectChunk` -> `Stream.fromIterableEffectRepeat`: Renamed; the effect now produces an Iterable/array instead of a Chunk, repeated forever.

- `Stream.repeatEffectChunkOption` -> `Stream.fromIterableEffectRepeat`: The Option\<E\> error encoding is gone: end the stream by failing the effect with `Cause.done()` (a Cause.Done failure); Done is excluded from the resulting stream's error type (Pull.ExcludeDone\<E\>).

- `Stream.repeatEffectOption` -> `Stream.fromEffectRepeat`: The Option\<E\> error encoding is gone: fail the effect with `Cause.done()` instead of Option.none() to end the stream; other failures propagate as stream errors.

- `Stream.repeatEffectWithSchedule` -> `Stream.fromEffectSchedule`: Renamed; runs the effect once, then repeats it per the schedule, emitting each result.

- `Stream.repeatEither` -> `none`: Removed; v4 Stream.repeat(schedule) repeats the stream but never emits the schedule outputs, and no Either/unification variant exists. If schedule outputs must be observed, hand-roll with Channel or track them via a schedule that taps into a Ref.

- `Stream.repeatElementsWith` -> `none`: Removed; v4 Stream.repeatElements(schedule) repeats each element per the schedule but never emits schedule outputs — the onElement/onSchedule unification is gone. Use repeatElements if only element repetition is needed.

- `Stream.repeatValue` -> `Stream.fromEffectRepeat`: Removed; use `Stream.fromEffectRepeat(Effect.succeed(value))` or `Stream.forever(Stream.succeed(value))`.

- `Stream.repeatWith` -> `none`: Removed; v4 Stream.repeat(schedule) covers the repetition but drops the schedule outputs and the onElement/onSchedule unification. Hand-roll if schedule outputs must appear in the stream.

- `Stream.runFoldScoped` -> `Stream.runFold`: Scoped run variants are gone; v4 run functions manage the stream's scope internally and the initial value is now a LazyArg: `Stream.runFold(self, () => s, f)`. For enclosing-scope control, pull manually via `Stream.toPull` (Effect\<Pull, never, R | Scope\>).

- `Stream.runFoldScopedEffect` -> `Stream.runFoldEffect`: Scoped run variants are gone; use `Stream.runFoldEffect(self, () => s, f)` — scope is managed internally, initial value is a LazyArg. Use Stream.toPull for manual scoped consumption.

#### `Stream.runFoldWhile`

**Replacement:** `none`

v4 runFold has no early-exit predicate; emulate with Stream.runForEachWhile and a mutable accumulator.

**Example**

```ts
// v3: Stream.runFoldWhile(self, init, cont, f)
Effect.suspend(() => {
  let acc = init
  return Stream.runForEachWhile(self, (a) => {
    acc = f(acc, a)
    return Effect.succeed(cont(acc))
  }).pipe(Effect.map(() => acc))
})

```

- `Stream.runFoldWhileEffect` -> `none`: v4 runFoldEffect has no early-exit predicate; emulate with Stream.runForEachWhile and a mutable accumulator, mapping the effectful step to Effect\<boolean\> via cont(acc) (see runFoldWhile example).

- `Stream.runFoldWhileScoped` -> `none`: Both the while-predicate and the scoped run variants are gone in v4; emulate the predicate with Stream.runForEachWhile plus a mutable accumulator (see runFoldWhile); scope is managed internally by v4 run functions.

- `Stream.runFoldWhileScopedEffect` -> `none`: Both the while-predicate and the scoped run variants are gone in v4; emulate with Stream.runForEachWhile plus a mutable accumulator and effectful step; scope is managed internally by v4 run functions.

- `Stream.runForEachChunk` -> `Stream.runForEachArray`: Renamed; the callback receives a NonEmptyReadonlyArray instead of a Chunk.

- `Stream.runForEachChunkScoped` -> `Stream.runForEachArray`: Scoped run variants are gone; v4 runForEachArray manages the stream scope internally. Use Stream.toPull for manual scoped consumption.

- `Stream.runForEachScoped` -> `Stream.runForEach`: Scoped run variants are gone; v4 runForEach manages the stream scope internally. Use Stream.toPull for manual scoped consumption.

- `Stream.runForEachWhileScoped` -> `Stream.runForEachWhile`: Scoped run variants are gone; v4 runForEachWhile (callback returns Effect\<boolean\>) manages the stream scope internally.

- `Stream.runIntoPubSub` -> `Stream.runIntoPubSub`: Pass a PubSub of plain elements rather than Take wrappers. Both overloads return Effect\<void, E, R\>, so handle stream failures on the returned Effect instead of expecting them as PubSub messages. The optional shutdownOnEnd flag controls PubSub shutdown.

- `Stream.runIntoPubSubScoped` -> `Stream.runIntoPubSub`: Scoped variant removed; v4 runIntoPubSub(pubsub, { shutdownOnEnd? }) publishes plain values (the Take wrapper is gone) and does not require Scope — fork the returned effect (Effect.forkIn/Effect.forkScoped) to reproduce the background scoped behavior.

- `Stream.runIntoQueueElementsScoped` -> `Stream.runIntoQueue`: The per-element Exit\<A, Option\<E\>\> encoding is gone; v4 runIntoQueue targets a Queue\<A, E | Cause.Done\> — elements are offered plainly and failure/end are signalled through the queue's error/done channel. Fork with Effect.forkIn for scoped background running.

- `Stream.runIntoQueueScoped` -> `Stream.runIntoQueue`: Scoped variant removed; v4 runIntoQueue offers plain values to a Queue\<A, E | Cause.Done\> (Take wrapper gone) and requires no Scope — fork the returned effect into a scope (Effect.forkIn) if needed.

- `Stream.runScoped` -> `Stream.run`: Scoped variant removed; v4 Stream.run(sink) manages the stream's scope internally. For consumption tied to an enclosing Scope, use Stream.toPull and drive the Pull manually.

- `Stream.scan` -> `Stream.scan`: Wrap the initial state in a thunk: Stream.scan(() =\> initial, step). The thunk is evaluated for each stream run; allocate mutable initial state inside it to avoid sharing state between runs.

- `Stream.scanEffect` -> `Stream.scanEffect`: Wrap the initial state in a thunk: Stream.scanEffect(() =\> initial, step). It is evaluated for each stream run; the step still returns an Effect.

- `Stream.scanReduce` -> `Stream.mapAccum`: Removed; emulate first-element-as-seed with `Stream.mapAccum(self, () => undefined as A | undefined, (acc, a) => { const next = acc === undefined ? a : f(acc, a); return [next, [next]] })`.

- `Stream.scanReduceEffect` -> `Stream.mapAccumEffect`: Removed; same first-element-as-seed emulation as scanReduce but with Stream.mapAccumEffect and an effectful step.

- `Stream.scheduleWith` -> `none`: Removed; v4 Stream.schedule(schedule) only paces elements and never emits schedule outputs — the onElement/onSchedule unification is gone. Use Stream.schedule if only pacing is needed.

- `Stream.scopedWith` -> `Stream.scoped`: Removed; v4 Stream.scoped scopes a Stream (provides a Scope kept open for the stream's lifetime). Emulate: `Stream.scoped(Stream.fromEffect(Effect.flatMap(Effect.scope, f)))` — Effect.scope accesses the ambient Scope.

- `Stream.some` -> `none`: Removed along with Option\<E\> error encodings. To drop None values use `Stream.filterMap(self, Filter.fromPredicateOption((o) => o))`; to fail on None use Stream.mapEffect with Option.match into Effect.fail/Effect.succeed.

- `Stream.someOrElse` -> `Stream.map`: Removed; use `Stream.map(self, Option.getOrElse(() => fallback()))`.

- `Stream.someOrFail` -> `Stream.mapEffect`: Removed; use `Stream.mapEffect(self, Option.match({ onNone: () => Effect.fail(error()), onSome: Effect.succeed }))`.

- `Stream.splitOnChunk` -> `none`: Delimiter-subsequence splitting was removed; v4 keeps only Stream.split (predicate/refinement, emitting NonEmptyReadonlyArray segments) and Stream.splitLines. Hand-roll multi-element delimiter splitting with Stream.mapAccumArray.

- `Stream.tapErrorCause` -> `Stream.tapCause`: Renamed; taps the full Cause on failure.

- `Stream.timeoutFail` -> `Stream.timeoutOrElse`: Use `Stream.timeoutOrElse(self, { duration, orElse: () => Stream.fail(error()) })`; the timeout resets on every emitted value as before.

- `Stream.timeoutFailCause` -> `Stream.timeoutOrElse`: Use `Stream.timeoutOrElse(self, { duration, orElse: () => Stream.failCause(cause()) })`.

- `Stream.timeoutTo` -> `Stream.timeoutOrElse`: Renamed into an options form: `Stream.timeoutOrElse(self, { duration, orElse: () => that })` — the fallback stream is now lazy.

- `Stream.toAsyncIterableRuntime` -> `Stream.toAsyncIterableWith`: Renamed; takes a `Context.Context<XR>` instead of a Runtime (v4 removed Runtime — a services Context is the execution environment). toAsyncIterable/toAsyncIterableEffect also still exist.

- `Stream.toQueueOfElements` -> `Stream.toQueue`: The Exit\<A, Option\<E\>\>-per-element queue is gone; v4 toQueue(options: { capacity, strategy? }) returns Effect\<Queue.Dequeue\<A, E | Cause.Done\>, never, R | Scope\> — elements are plain values and failure/end arrive through the queue's error/done channel.

- `Stream.toReadableStreamRuntime` -> `Stream.toReadableStreamWith`: Renamed; takes a `Context.Context<XR>` instead of a Runtime (v4 removed Runtime); options `{ strategy?: QueuingStrategy }` unchanged.

- `Stream.transduce` -> `Stream.transduce`: Unchanged name and Sink-based shape; chunks are plain arrays in v4.

- `Stream.unfoldChunk` -> `Stream.paginate`: Removed; v4 Stream.paginate(s, (s) =\> Effect\<[ReadonlyArray\<A\>, Option\<S\>]\>) is the array-emitting unfold — wrap the pure step in Effect.succeed; to end without emitting return `[[], Option.none()]`.

- `Stream.unfoldChunkEffect` -> `Stream.paginate`: Removed; v4 Stream.paginate has the effectful array-step shape — map v3's Option\<[Chunk, S]\> result to `[array, Option<S>]`, returning `[[], Option.none()]` to end without emitting.

- `Stream.unfoldEffect` -> `Stream.unfold`: v4 Stream.unfold is effectful: `unfold(s, (s) => Effect<readonly [A, S] | undefined>)` — return the pair or `undefined` to end instead of Option.

- `Stream.unwrapScoped` -> `Stream.unwrap`: v4 Stream.unwrap accepts scoped effects (`Exclude<R, Scope>` built in); the scope stays open for the stream's lifetime — it replaces both unwrap and unwrapScoped.

- `Stream.unwrapScopedWith` -> `Stream.unwrap`: Removed; access the ambient Scope explicitly: `Stream.unwrap(Effect.flatMap(Effect.scope, f))` — v4 unwrap keeps the scope open for the stream's lifetime.

- `Stream.void` -> `Stream.succeed(void 0)`: The `Stream.void` constant (single void element) was removed; use `Stream.succeed(void 0)` or `Stream.make(void 0)`.

- `Stream.whenCase` -> `none`: Removed; emulate with `Stream.suspend(() => Option.match(pf(evaluate()), { onNone: () => Stream.empty, onSome: (s) => s }))`.

- `Stream.whenCaseEffect` -> `none`: Removed; emulate with `Stream.unwrap(Effect.map(self, (a) => Option.match(pf(a), { onNone: () => Stream.empty, onSome: (s) => s })))`.

- `Stream.whenEffect` -> `Stream.when`: Folded into Stream.when, which now takes an `Effect<boolean>` test directly (wrap a pure condition with Effect.sync).

- `Stream.zipAll` -> `none`: The entire zipAll family was removed in v4 (only zip/zipLatest/zipLatestAll exist; zip ends at the shorter side, zipLatest\* combine latest values — different semantics). Pad-with-default zipping must be hand-rolled, e.g. with Stream.combineArray pulling both sides.

- `Stream.zipAllLeft` -> `none`: Removed with the zipAll family; no default-padding zip exists in v4. Hand-roll with Stream.combineArray (or concat the remainder after a plain Stream.zipLeft) if needed.

- `Stream.zipAllRight` -> `none`: Removed with the zipAll family; no default-padding zip exists in v4. Hand-roll with Stream.combineArray if needed.

- `Stream.zipAllSortedByKey` -> `none`: Removed; the sorted-by-key merge-join family has no v4 equivalent (checked v4 Stream exports — only zip/zipLatest/zipLatestAll/zipWithArray). Hand-roll a keyed merge with Stream.combineArray.

- `Stream.zipAllSortedByKeyLeft` -> `none`: Removed; see zipAllSortedByKey — no keyed merge-join in v4, hand-roll with Stream.combineArray.

- `Stream.zipAllSortedByKeyRight` -> `none`: Removed; see zipAllSortedByKey — no keyed merge-join in v4, hand-roll with Stream.combineArray.

- `Stream.zipAllSortedByKeyWith` -> `none`: Removed; see zipAllSortedByKey — no keyed merge-join in v4, hand-roll with Stream.combineArray.

- `Stream.zipAllWith` -> `none`: Removed with the zipAll family; v4 has no zip that pads the shorter side with defaults. Hand-roll with Stream.combineArray.

- `Stream.zipWithChunks` -> `Stream.zipWithArray`: Renamed; the combiner now receives two NonEmptyReadonlyArrays and returns `[output: NonEmptyReadonlyArray, leftoverLeft: ReadonlyArray, leftoverRight: ReadonlyArray]` — the Either-wrapped leftover (ZipChunksResult) is replaced by the two explicit leftover arrays.
