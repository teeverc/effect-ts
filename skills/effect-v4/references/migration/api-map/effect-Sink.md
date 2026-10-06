# v3 → v4 API map: effect/Sink

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Sink`

- `Sink.Sink` -> `Sink`: Interface kept as Sink\<A, In, L, E, R\> with the same type parameters; the internal representation changed from a Channel wrapper to a `transform: (upstream: Pull<NonEmptyReadonlyArray<In>>, scope) => Effect<End<A, L>>` function, and completion is the tuple `Sink.End<A, L> = readonly [value, leftover?]`.

- `Sink.Sink.Variance` -> `Sink.Variance`: Still exists as the namespace interface Sink.Variance\<A, In, L, E, R\> (with Sink.VarianceStruct); the variance key is now the internal string "\~effect/Sink" instead of the SinkTypeId symbol.

- `Sink.SinkTypeId` -> `none`: The type id is the unexported internal string "\~effect/Sink" (no unique symbol, no export); use the new guard Sink.isSink(u) instead of checking the symbol.

- `Sink.SinkUnify` -> `SinkUnify`: Kept with the same name and shape (extends Effect.EffectUnify, preserves all five Sink type parameters).

- `Sink.SinkUnifyIgnore` -> `SinkUnifyIgnore`: Kept with the same name; now a standalone `{ Effect?: true }` interface instead of extending Effect.EffectUnifyIgnore.

- `Sink.collectAll` -> `Sink.collect`: Renamed; returns Sink\<Array\<In\>, In\> collecting into a plain mutable Array instead of Chunk.

- `Sink.collectAllFrom` -> `none`: Repeated-run result accumulation was removed; checked the v4 export list (no collectAllFrom/repeatedly). Re-implement with Sink.fromTransform, looping self.transform on the upstream pull (feeding leftovers back) until the upstream ends, accumulating results in an array.

- `Sink.collectAllN` -> `Sink.take`: Sink.take(n) returns Sink\<Array\<In\>, In, In\> collecting up to n elements (Array instead of Chunk), emitting the unconsumed remainder as leftovers.

- `Sink.collectAllToMap` -> `Sink.reduce`: Built-in HashMap collector removed; build a plain Map in the reducer: Sink.reduce(() =\> new Map\<K, In\>(), (m, in\_) =\> { const k = key(in\_); return m.set(k, m.has(k) ? merge(m.get(k)!, in\_) : in\_) }).

- `Sink.collectAllToMapN` -> `none`: Removed; approximating with Sink.reduceWhile((...) , (m) =\> m.size \< n, ...) consumes (merges) the element that introduces the (n+1)-th key, whereas v3 left it as leftover — exact v3 leftover behavior needs a custom Sink.fromTransform that checks the key before consuming.

- `Sink.collectAllToSet` -> `Sink.reduce`: Built-in HashSet collector removed; build a plain Set: Sink.reduce(() =\> new Set\<In\>(), (s, in\_) =\> s.add(in\_)).

- `Sink.collectAllToSetN` -> `Sink.reduceWhile`: Removed; equivalent via Sink.reduceWhile(() =\> new Set\<In\>(), (s) =\> s.size \< n, (s, in\_) =\> s.add(in\_)) — stops with leftovers once n distinct values are collected (plain Set instead of HashSet).

- `Sink.collectAllUntil` -> `Sink.takeUntil`: Renamed; Sink.takeUntil(predicate) collects into Array\<In\> until the predicate matches, including the matching element, like v3.

- `Sink.collectAllUntilEffect` -> `Sink.takeUntilEffect`: Renamed; Sink.takeUntilEffect(p) collects into Array\<In\> until the effectful predicate returns true, including the matching element.

- `Sink.collectAllWhile` -> `Sink.takeWhile`: Renamed; Sink.takeWhile(predicate) collects the matching prefix into Array\<In\>, keeps the refinement overload, and excludes the first failing element (returned via leftovers).

- `Sink.collectAllWhileEffect` -> `Sink.takeWhileEffect`: Renamed; Sink.takeWhileEffect(p) collects into Array\<In\> while the effectful predicate returns true.

- `Sink.collectAllWhileWith` -> `none`: Repeatedly-run-and-fold was removed (no v4 counterpart in the export list); re-implement with Sink.fromTransform looping self.transform while the `while` predicate holds on each result, folding results with `body` and feeding leftovers back into the next run.

- `Sink.collectLeftover` -> `Sink.mapEnd`: Use Sink.mapEnd to move the leftovers into the result: Sink.mapEnd(self, ([a, leftover]) =\> [[a, leftover ?? []] as const]); leftovers are NonEmptyReadonlyArray\<L\> | undefined instead of Chunk\<L\>.

- `Sink.context` -> `Sink.fromEffect(Effect.context())`: Sink.context was removed; Sink.fromEffect(Effect.context\<R\>()) yields the same Sink\<Context\<R\>, unknown, never, never, R\>.

- `Sink.contextWith` -> `Sink.fromEffect(Effect.contextWith(f))`: Removed; compose Sink.fromEffect with Effect.contextWith to derive a value from the context.

- `Sink.contextWithEffect` -> `Sink.fromEffect(Effect.flatMap(Effect.context(), f))`: Removed, and v4 Effect has no contextWithEffect; use Sink.fromEffect(Effect.flatMap(Effect.context\<R0\>(), f)).

- `Sink.contextWithSink` -> `Sink.unwrap(Effect.contextWith(f))`: Removed; Sink.unwrap(Effect.contextWith((ctx: Context.Context\<R0\>) =\> f(ctx))) builds the sink from the context.

- `Sink.dieMessage` -> `Sink.die`: Removed (v4 has no RuntimeException-based dieMessage anywhere); use Sink.die(new Error(message)).

- `Sink.dieSync` -> `Sink.failCauseSync`: Removed; use Sink.failCauseSync(() =\> Cause.die(evaluate())) to defer defect evaluation, or Sink.die(defect) when eager is fine.

- `Sink.dimap` -> `Sink.mapInput + Sink.map`: Removed; compose the two halves: self.pipe(Sink.mapInput(f), Sink.map(g)).

- `Sink.dimapChunks` -> `Sink.mapInputArray + Sink.map`: Removed; compose self.pipe(Sink.mapInputArray(f), Sink.map(g)) — f now maps NonEmptyReadonlyArray instead of Chunk and must return a non-empty array.

- `Sink.dimapChunksEffect` -> `Sink.mapInputArrayEffect + Sink.mapEffect`: Removed; compose self.pipe(Sink.mapInputArrayEffect(f), Sink.mapEffect(g)) — f maps NonEmptyReadonlyArray instead of Chunk and must return a non-empty array.

- `Sink.dimapEffect` -> `Sink.mapInputEffect + Sink.mapEffect`: Removed; compose self.pipe(Sink.mapInputEffect(f), Sink.mapEffect(g)).

- `Sink.drop` -> `none`: The drop\* sinks were removed (nothing in the v4 export list); drop on the stream side instead with Stream.drop(n) before running the sink, or write a Sink.fromTransform that discards the first n pulled elements.

- `Sink.dropUntil` -> `none`: Removed with the other drop\* sinks; use Stream.dropUntil(predicate) on the stream before running the sink.

- `Sink.dropUntilEffect` -> `none`: Removed; use Stream.dropUntilEffect(p) on the stream before running the sink.

- `Sink.dropWhile` -> `none`: Removed; use Stream.dropWhile(predicate) on the stream before running the sink.

- `Sink.dropWhileEffect` -> `none`: Removed; use Stream.dropWhileEffect(p) on the stream before running the sink.

- `Sink.ensuringWith` -> `Sink.onExit`: Renamed; Sink.onExit(self, (exit: Exit\<A, E\>) =\> finalizer) runs after completion, failure, or interruption — the exit now carries the sink's result value A (v3 passed Exit\<unknown, E\>). Plain Sink.ensuring(effect) also still exists for the exit-independent case.

- `Sink.filterInput` -> `none`: Removed, and not expressible via Sink.mapInputArray because its function must return a non-empty array (a fully-filtered batch is illegal); filter on the stream with Stream.filter(predicate) before running the sink, or write a Sink.fromTransform that skips empty filtered batches.

- `Sink.filterInputEffect` -> `none`: Removed (same non-empty-array constraint as filterInput); use Stream.filterEffect(p) on the stream before running the sink.

- `Sink.foldChunks` -> `Sink.reduceWhileArray`: Sink.reduceWhileArray(() =\> s, contFn, f) folds whole input batches; initial state is now a lazy thunk and f receives NonEmptyReadonlyArray\<In\> instead of Chunk\<In\>.

- `Sink.foldChunksEffect` -> `Sink.reduceWhileArrayEffect`: Sink.reduceWhileArrayEffect(() =\> s, contFn, f) is the effectful array-level fold with continuation predicate; lazy initial state, NonEmptyReadonlyArray instead of Chunk. Sink.foldArray has the same shape but does not check contFn on the initial state.

- `Sink.foldEffect` -> `Sink.reduceWhileEffect`: Sink.reduceWhileEffect(() =\> s, contFn, f) folds element-by-element with an effectful step and continuation predicate (checked on the initial state, like v3); initial state is now a lazy thunk. v4 Sink.fold has the same signature but skips the initial-state check.

- `Sink.foldLeft` -> `Sink.reduce`: Renamed; Sink.reduce(() =\> s, f) — initial state is now a lazy thunk, semantics otherwise identical.

- `Sink.foldLeftChunks` -> `Sink.reduceArray`: Renamed; Sink.reduceArray(() =\> s, f) folds whole batches — lazy initial state, f receives NonEmptyReadonlyArray\<In\> instead of Chunk\<In\>.

- `Sink.foldLeftChunksEffect` -> `Sink.reduceWhileArrayEffect`: No plain reduceArrayEffect exists in v4; use Sink.reduceWhileArrayEffect(() =\> s, () =\> true, f) (constant-true predicate) — f receives NonEmptyReadonlyArray\<In\> instead of Chunk\<In\> and the result has L = never like v3.

- `Sink.foldLeftEffect` -> `Sink.reduceEffect`: Renamed; Sink.reduceEffect(() =\> s, f) — lazy initial state, effectful step, no termination predicate.

- `Sink.foldSink` -> `Sink.orElse + Sink.flatMap`: The two-channel match was removed; compose self.pipe(Sink.orElse((e) =\> options.onFailure(e)), Sink.flatMap((a) =\> options.onSuccess(a))) — orElse switches to the failure sink (resuming the same upstream), flatMap feeds leftovers to the success sink first.

- `Sink.foldUntilEffect` -> `Sink.foldUntil`: v4 Sink.foldUntil(() =\> s, max, f) takes the effectful step function directly (f returns Effect\<S, E, R\>), so it covers v3 foldUntilEffect; initial state is now a lazy thunk. For the pure v3 foldUntil wrap the step in Effect.succeed.

- `Sink.foldWeighted` -> `none`: The whole foldWeighted family was removed from v4 Sink (checked the export list); re-implement with Sink.fold carrying the accumulated cost in the state (cont while cost \< max), returning leftovers automatically when stopping mid-batch.

- `Sink.foldWeightedDecompose` -> `none`: Removed with no decompose mechanism in v4; splitting oversized elements must happen upstream (transform the stream before the sink) or inside a custom Sink.fromTransform.

- `Sink.foldWeightedDecomposeEffect` -> `none`: Removed; same as foldWeightedDecompose — no effectful weighted/decompose fold exists, re-implement via Sink.fromTransform or restructure upstream.

- `Sink.foldWeightedEffect` -> `none`: Removed; re-implement with Sink.fold (its step is effectful in v4) tracking accumulated cost in the state.

- `Sink.forEachChunk` -> `Sink.forEachArray`: Renamed; f receives NonEmptyReadonlyArray\<In\> instead of Chunk\<In\>.

- `Sink.forEachChunkWhile` -> `Sink.forEachWhileArray`: Renamed; f: (NonEmptyReadonlyArray\<In\>) =\> Effect\<boolean\> continues while true, stops on false, as in v3.

- `Sink.fromPush` -> `Sink.fromTransform`: The push-based protocol (Option\<Chunk\> push function failing with [Either, leftovers]) is gone; v4's low-level constructor is pull-based: Sink.fromTransform((upstream: Pull\<NonEmptyReadonlyArray\<In\>\>, scope) =\> Effect\<End\<A, L\>\>) — pull inputs from upstream and finish by succeeding with the [value, leftover?] tuple.

- `Sink.leftover` -> `Sink.succeed`: Removed as a standalone constructor; Sink.succeed now takes optional leftovers: Sink.succeed(void 0, leftovers) where leftovers is a NonEmptyReadonlyArray\<L\> instead of Chunk\<L\>.

- `Sink.mapInputChunks` -> `Sink.mapInputArray`: Renamed; f maps NonEmptyReadonlyArray\<In0\> =\> NonEmptyReadonlyArray\<In\> (must stay non-empty) instead of Chunk =\> Chunk.

- `Sink.mapInputChunksEffect` -> `Sink.mapInputArrayEffect`: Renamed; f maps NonEmptyReadonlyArray\<In0\> =\> Effect\<NonEmptyReadonlyArray\<In\>\> (must stay non-empty) instead of Chunk =\> Effect\<Chunk\>.

- `Sink.mkString` -> `Sink.reduceArray`: Removed as a built-in; equivalent one-liner: Sink.reduceArray(() =\> "", (s, arr) =\> s + arr.join("")).

- `Sink.race` -> `none`: Sink racing (race/raceBoth/raceWith) was removed from v4; broadcast the stream (Stream.broadcast) into two consumers and race the resulting run effects with Effect.race, or write a custom Channel.

- `Sink.raceBoth` -> `none`: Removed with the race family; broadcast the stream and use Effect.raceBoth (or Effect.race) on the two Stream.run effects to learn which side won.

- `Sink.raceWith` -> `none`: Removed, along with the MergeDecision type it depended on; the closest is broadcasting the stream and combining the two run effects manually (Effect.raceWith on the run effects).

- `Sink.refineOrDie` -> `Sink.catch`: Removed; rebuild with the typed-error handler: Sink.catch(self, (e) =\> Option.match(pf(e), { onSome: Effect.fail, onNone: () =\> Effect.die(e) })) — note Sink.catch replaces the result on recovery, so refined errors must be re-failed as shown.

- `Sink.refineOrDieWith` -> `Sink.catch`: Removed; same pattern as refineOrDie but die with the mapped defect: Sink.catch(self, (e) =\> Option.match(pf(e), { onSome: Effect.fail, onNone: () =\> Effect.die(f(e)) })).

- `Sink.splitWhere` -> `none`: Removed; it re-chunked input so the sink stopped before the first later element matching the predicate — closest v4 options are pre-splitting the stream (Stream.split / Stream.takeWhile) or a custom Sink.fromTransform that cuts pulled arrays at the predicate boundary and returns the rest as leftovers.

- `Sink.unwrapScoped` -> `Sink.unwrap`: Folded into Sink.unwrap, whose signature now excludes Scope from R (Sink\<..., Exclude\<R, Scope\> | R2\>), so scoped effects are accepted directly; resources stay open for the sink's lifetime.

- `Sink.unwrapScopedWith` -> `Sink.unwrap`: Folded into Sink.unwrap — obtain the scope inside the effect via Effect.scope (Sink.unwrap(Effect.flatMap(Effect.scope, f))); for direct scope access use Sink.fromTransform, whose transform receives (upstream, scope).

- `Sink.zip` -> `Sink.flatMap`: The zip family was removed; sequential zip is self.pipe(Sink.flatMap((a) =\> Sink.map(that, (a2) =\> [a, a2] as const))) — leftovers of the first sink feed the second. The { concurrent: true } racing mode has no v4 equivalent.

- `Sink.zipLeft` -> `Sink.flatMap`: Removed; use self.pipe(Sink.flatMap((a) =\> Sink.as(that, a))) to run both sequentially and keep the first result (no concurrent option).

- `Sink.zipRight` -> `Sink.flatMap`: Removed; use self.pipe(Sink.flatMap(() =\> that)) to run both sequentially and keep the second result (no concurrent option).

- `Sink.zipWith` -> `Sink.flatMap`: Removed; use self.pipe(Sink.flatMap((a) =\> Sink.map(that, (a2) =\> f(a, a2)))) — sequential only, the { concurrent: true } option has no v4 equivalent.
