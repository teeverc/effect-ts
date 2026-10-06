# v3 → v4 API map: effect/Channel

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Channel`

- `Channel.Channel` -> `Channel.Channel`: Retained, but reorder type parameters from \<OutElem, InElem, OutErr, InErr, OutDone, InDone, Env\> to \<OutElem, OutErr, OutDone, InElem, InErr, InDone, Env\>. Convert Effect values explicitly with Channel.fromEffect or Channel.fromEffectDone.

- `Channel.ChannelException` -> `none`: Removed implementation artifact. Channel.pipeToOrFail now handles upstream failures without exposing the v3 exception wrapper.

- `Channel.ChannelExceptionTypeId` -> `none`: Removed implementation artifact. Channel.pipeToOrFail now handles upstream failures without exposing the v3 exception wrapper.

- `Channel.ChannelTypeId` -> `Channel.TypeId`: Renamed to TypeId; the brand is now the string literal \~effect/Channel. Prefer Channel.isChannel for runtime checks.

- `Channel.ChannelUnify` -> `Channel.ChannelUnify`: Retained; update inferred Channel arguments to the v4 generic order.

- `Channel.ChannelUnifyIgnore` -> `Channel.ChannelUnifyIgnore`: Retained with a new shape: it no longer extends EffectUnifyIgnore and now contains Effect?: true.

- `Channel.acquireReleaseOut` -> `Channel.acquireRelease`: Renamed to acquireRelease. The v4 release action cannot add environment requirements, so capture or provide any services it needs.

- `Channel.as` -> `Channel.mapDone`: Replace Channel.as(self, value) with Channel.mapDone(self, () =\> value).

- `Channel.asVoid` -> `Channel.mapDone`: Replace Channel.asVoid(self) with Channel.mapDone(self, () =\> void 0).

- `Channel.bufferChunk` -> `none`: The inferred Channel.fromChunk match is not equivalent. Rebuild the buffered upstream-pull transform with Channel.fromTransform and Channel.toTransform.

- `Channel.catchAll` -> `Channel.catch`: Renamed to catch for typed-error recovery.

- `Channel.catchAllCause` -> `Channel.catchCause`: Renamed to catchCause for full-cause recovery.

- `Channel.collect` -> `Channel.filterMap`: Use Channel.filterMap(self, Filter.fromPredicateOption(pf)) to adapt the v3 Option-returning partial function.

- `Channel.concatAll` -> `Channel.flatten`: Use flatten for sequential emitted-channel flattening. V4 preserves the outer done value and discards child done values.

- `Channel.concatAllWith` -> `none`: V4 removed child-done accumulation and the outer-done combiner. Use Channel.flatten or Channel.flatMap only when child done values may be discarded; otherwise implement a Pull transform.

- `Channel.concatMap` -> `Channel.flatMap`: Renamed to flatMap. Sequential flattening is the default; child done values are discarded and the source done value is preserved.

- `Channel.concatMapWith` -> `none`: V4 removed child-done accumulation and the outer-done combiner. Use Channel.flatten or Channel.flatMap only when child done values may be discarded; otherwise implement a Pull transform.

- `Channel.concatMapWithCustom` -> `none`: Removed with the channel executor scheduling protocol. Use Channel.flatMap for ordinary sequencing or implement custom scheduling with Channel.fromTransform and Pull.

- `Channel.concatOut` -> `Channel.flatten`: Use flatten for sequential emitted-channel flattening. V4 preserves the outer done value and discards child done values.

- `Channel.context` -> `Channel.contextWith`: Use Channel.contextWith((context) =\> Channel.end(context)); the context was the v3 channel done value.

- `Channel.contextWithChannel` -> `Channel.contextWith`: Renamed to contextWith.

- `Channel.contextWithEffect` -> `Channel.contextWith`: Use Channel.contextWith((context) =\> Channel.fromEffectDone(f(context))) to preserve the effect result as the done value.

- `Channel.doneCollect` -> `none`: No exact channel combinator remains. Drive Channel.toPull, collect output elements, and handle Cause.Done to retain both outputs and the done value.

- `Channel.emitCollect` -> `none`: No exact channel combinator remains. Drive Channel.toPull, collect output elements, and handle Cause.Done to retain both outputs and the done value.

- `Channel.ensuringWith` -> `Channel.onExit`: Renamed to onExit; the finalizer still receives the channel Exit.

- `Channel.foldCauseChannel` -> `none`: V4 has no exact two-sided fold over failure and completion. Use catchCause or catch for failure-only handling, concatWith for success-only handling, or match the Pull in a custom transform.

- `Channel.foldChannel` -> `none`: V4 has no exact two-sided fold over failure and completion. Use catchCause or catch for failure-only handling, concatWith for success-only handling, or match the Pull in a custom transform.

- `Channel.fromEither` -> `Channel.fromEffectDone`: Either is now Result. Use Channel.fromEffectDone(Effect.fromResult(result)) to preserve success as the done value.

- `Channel.fromInput` -> `none`: SingleProducerAsyncInput was removed. Model the producer with Queue and Pull; use Channel.fromPull with Queue.take when a typed done value matters.

- `Channel.fromOption` -> `Channel.fromEffectDone`: Use Channel.fromEffectDone(Effect.fromOption(option, Option.none)) to preserve the v3 Option.none error, or omit onNone for the v4 NoSuchElementError default.

- `Channel.fromPubSubScoped` -> `Channel.fromPubSubTake`: Change the protocol to PubSub\<Take\<Elem, Err, Done\>\> and use Channel.flattenArray(Channel.fromPubSubTake(pubsub)). The v4 constructor owns the scoped subscription and returns a Channel directly.

- `Channel.interruptWhenDeferred` -> `Channel.interruptWhen`: Use Channel.interruptWhen(self, Deferred.await(deferred)); the Deferred-specific overload was removed.

- `Channel.isChannelException` -> `none`: Removed implementation artifact. Channel.pipeToOrFail now handles upstream failures without exposing the v3 exception wrapper.

- `Channel.mapErrorCause` -> `Channel.catchCause`: Use Channel.catchCause(self, (cause) =\> Channel.failCause(f(cause))).

- `Channel.mapInputContext` -> `Channel.updateContext`: Renamed to updateContext for transforming the channel requirement Context.

- `Channel.mapInputEffect` -> `none`: V4 removed upstream done/error effect mapping. Adapt Cause.Done or failure on the upstream Pull, then pass it through Channel.toTransform(self).

- `Channel.mapInputErrorEffect` -> `none`: V4 removed upstream done/error effect mapping. Adapt Cause.Done or failure on the upstream Pull, then pass it through Channel.toTransform(self).

- `Channel.mapInputIn` -> `Channel.mapInput`: Use Channel.mapInput(self, (value) =\> Effect.succeed(f(value))); v4 consolidated pure and effectful input mapping.

- `Channel.mapInputInEffect` -> `Channel.mapInput`: Renamed to mapInput; the mapper remains effectful.

- `Channel.mapOut` -> `Channel.map`: Renamed to map; the v4 mapper also receives the element index.

- `Channel.mapOutEffect` -> `Channel.mapEffect`: Renamed to mapEffect for sequential effectful output mapping.

- `Channel.mapOutEffectPar` -> `Channel.mapEffect`: Use Channel.mapEffect(self, f, { concurrency: n }); ordered output remains the default.

- `Channel.mergeAllUnbounded` -> `Channel.mergeAll`: Use Channel.mergeAll(channels, { concurrency: "unbounded" }); child done values are discarded and the outer done value is preserved.

- `Channel.mergeAllUnboundedWith` -> `none`: V4 mergeAll removed child-done combining. Use Channel.mergeAll if terminal accumulation can be dropped; otherwise implement a custom Pull merge.

- `Channel.mergeAllWith` -> `none`: V4 mergeAll removed child-done combining. Use Channel.mergeAll if terminal accumulation can be dropped; otherwise implement a custom Pull merge.

- `Channel.mergeMap` -> `Channel.flatMap`: Use Channel.flatMap with concurrency and bufferSize for backpressure, or Channel.switchMap with the same options for the v3 sliding strategy.

- `Channel.mergeOut` -> `Channel.mergeAll`: Use Channel.mergeAll(self, { concurrency: n }) for bounded backpressured flattening when child done values are irrelevant.

- `Channel.mergeOutWith` -> `none`: V4 mergeAll removed child-done combining. Use Channel.mergeAll if terminal accumulation can be dropped; otherwise implement a custom Pull merge.

- `Channel.mergeWith` -> `Channel.merge`: Use Channel.merge with haltStrategy left, right, both, or either for standard policies. Custom MergeDecision effects require a Pull-level redesign.

- `Channel.orDieWith` -> `Channel.catch`: Use Channel.catch(self, (error) =\> Channel.die(f(error))); v4 Channel.orDie has no mapping callback.

- `Channel.orElse` -> `Channel.catch`: Use Channel.catch(self, () =\> that()) and keep the fallback lazy.

- `Channel.provideLayer` -> `Channel.provide`: Both collapse into provide. V4 removes services supplied by the layer and retains remaining requirements; use options.local when a fresh layer instance is needed.

- `Channel.provideSomeLayer` -> `Channel.provide`: Both collapse into provide. V4 removes services supplied by the layer and retains remaining requirements; use options.local when a fresh layer instance is needed.

- `Channel.read` -> `none`: The channel instruction AST was removed. Implement a one-step upstream read with Channel.fromTransform and Pull matching, including Cause.Done handling.

- `Channel.readOrFail` -> `none`: The channel instruction AST was removed. Implement a one-step upstream read with Channel.fromTransform and Pull matching, including Cause.Done handling.

- `Channel.readWith` -> `none`: The channel instruction AST was removed. Implement a one-step upstream read with Channel.fromTransform and Pull matching, including Cause.Done handling.

- `Channel.readWithCause` -> `none`: The channel instruction AST was removed. Implement a one-step upstream read with Channel.fromTransform and Pull matching, including Cause.Done handling.

- `Channel.repeated` -> `Channel.forever`: Use forever for infinite repetition. Channel.repeat takes a Schedule and may terminate, so it is not equivalent.

- `Channel.run` -> `Channel.runDrain`: Use runDrain to consume all emitted elements and return the channel's done value.

- `Channel.runScoped` -> `Channel.toPull`: No direct scoped runner remains. Use toPull in the caller scope and recover Cause.Done; use runDrain when an internally managed scope is acceptable.

- `Channel.scopedWith` -> `Channel.unwrap`: Use Channel.unwrap(Effect.map(Effect.scope, (scope) =\> Channel.fromEffect(f(scope)))) so the effect uses the active channel scope.

- `Channel.toPullIn` -> `Channel.toPullScoped`: Renamed to toPullScoped. The returned Pull emits elements directly and represents completion with Cause.Done instead of Either.

- `Channel.toSink` -> `Sink.fromChannel`: Constructor moved to Sink. Adapt the channel to non-empty array input, no emitted leftovers, and a Sink.End done value.

- `Channel.toStream` -> `Stream.fromChannel`: Constructor moved to Stream. Adapt Chunk outputs to non-empty readonly arrays and map the channel done value to void.

- `Channel.unwrapScoped` -> `Channel.unwrap`: Use unwrap; v4 supplies the active channel scope to the effect and removes Scope from the resulting requirement.

- `Channel.unwrapScopedWith` -> `Channel.unwrap`: Use Channel.unwrap(Effect.flatMap(Effect.scope, f)) to pass the active channel scope to f.

- `Channel.void` -> `Channel.empty`: Renamed to empty: emit nothing and end with void.

- `Channel.write` -> `Channel.succeed`: Renamed to succeed, which emits one element in v4. Use Channel.end when migrating v3 succeed, which produced a done value.

- `Channel.writeAll` -> `Channel.fromArray`: Replace the variadic writer with Channel.fromArray(outs).

- `Channel.writeChunk` -> `Channel.fromChunk`: Renamed to fromChunk for emitting every Chunk element.

- `Channel.zip` -> `Channel.concatWith`: For sequential zip, concatWith the left channel and mapDone the right result to a tuple. Concurrent tuple-done semantics require custom Pull coordination.

- `Channel.zipLeft` -> `Channel.concatWith`: For sequential zipLeft, concatWith and mapDone the right result back to the left done value. Concurrent done preservation requires custom Pull coordination.

- `Channel.zipRight` -> `Channel.concat`: Use concat for the sequential form; it preserves the right done value. Concurrent mode has no exact replacement.
