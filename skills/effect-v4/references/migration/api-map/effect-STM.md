# v3 → v4 API map: effect/STM

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/STM`

- `STM.Adapter` -> `none`: The STM.gen adapter was removed; Effect.gen accepts yielded Effects directly.

- `STM.All.IsDiscard` -> `Effect.All.IsDiscard`: The helper moved to Effect.All because STM.all is now Effect.all.

- `STM.All.Narrow` -> `none`: Effect.all uses a const generic directly, so the separate tuple-narrowing helper was removed.

- `STM.All.Options` -> `none`: Effect.all inlines its options type; use its concurrency, discard, and mode options directly.

- `STM.All.STMAny` -> `Effect.All.EffectAny`: STM inputs are ordinary Effects in v4, so use the Effect.All helper.

- `STM.All.Signature` -> `typeof Effect.all`: The named STM all signature was removed; refer to Effect.all directly.

- `STM.Do` -> `Effect.Do`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.STM` -> `Effect.Effect`: The distinct STM instruction type was removed. Tx APIs return Effect values; wrap the complete transaction in Effect.tx.

- `STM.STM.Variance` -> `Effect.Variance`: The distinct STM variance marker was removed with STM; use the Effect marker.

- `STM.STMTypeId` -> `Effect.TypeId`: The distinct STM type id was removed because transactions are represented by Effect values.

- `STM.STMTypeLambda` -> `Effect.EffectTypeLambda`: Use the Effect type lambda; transactional requirements are represented by Effect.Transaction.

- `STM.STMUnify` -> `Effect.EffectUnify`: STM unification moved to ordinary Effect unification.

- `STM.STMUnifyIgnore` -> `none`: The STM-specific unification ignore marker was removed; rely on Effect inference.

- `STM.acquireUseRelease` -> `Effect.acquireUseRelease + Effect.tx`: Wrap acquire, use, and release in separate Effect.tx calls to preserve the v3 separately committed phases; v4 release also receives the use Exit.

- `STM.all` -> `Effect.all`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.as` -> `Effect.as`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.asSome` -> `Effect.asSome`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.asSomeError` -> `Effect.mapError(self, Option.some)`: The dedicated helper was removed; map the error into Option.some.

- `STM.asVoid` -> `Effect.asVoid`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.attempt` -> `Effect.try`: The constructor was renamed; transaction programs are ordinary Effects in v4.

- `STM.bind` -> `Effect.bind`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.bindTo` -> `Effect.bindTo`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.catchAll` -> `Effect.catch`: Use Effect.catch for typed failures. It does not catch Effect.txRetry or restore a transactional savepoint.

- `STM.catchSome` -> `Effect.catch + Option.match`: Use Effect.catch and re-fail the original error when the partial handler returns None.

- `STM.catchTag` -> `Effect.catchTag`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.check` -> `Effect.suspend + Effect.txRetry`: Evaluate the predicate lazily and return Effect.void when true or Effect.txRetry when false, inside Effect.tx.

- `STM.collect` -> `Effect.flatMap + Option.match + Effect.txRetry`: Map Some to success and None to Effect.txRetry inside the surrounding Effect.tx transaction.

- `STM.collectSTM` -> `Effect.flatMap + Option.match + Effect.txRetry`: Return the Effect held by Some and use Effect.txRetry for None, inside the surrounding Effect.tx transaction.

- `STM.commit` -> `Effect.tx`: Effect.tx runs an Effect transaction and removes its Effect.Transaction requirement.

- `STM.commitEither` -> `Effect.tx + Effect.result + Effect.fromResult`: Run Effect.tx(Effect.result(body)) before Effect.fromResult so journal changes commit even when the original transaction had a typed failure.

- `STM.cond` -> `Effect.suspend`: Lazily branch to Effect.succeed or Effect.fail based on the predicate.

- `STM.context` -> `Effect.context`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.contextWith` -> `Effect.contextWith`: The name remains, but the v4 callback returns an Effect directly.

- `STM.contextWithSTM` -> `Effect.contextWith`: The Effect-returning context constructor no longer needs an STM suffix.

- `STM.dieMessage` -> `Effect.die(new Error(message))`: The message-specific helper was removed; construct a message-bearing defect explicitly.

- `STM.dieSync` -> `Effect.suspend(() => Effect.die(evaluate()))`: The lazy defect helper was removed; suspend construction and then die.

- `STM.either` -> `Effect.result`: V4 uses Result instead of Either for materialized typed failures.

- `STM.ensuring` -> `Effect.ensuring`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.eventually` -> `Effect.eventually`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.every` -> `Effect.findFirst + Option.isNone`: Search sequentially for the first false effectful predicate; no match means every element passed.

- `STM.exists` -> `Effect.findFirst + Option.isSome`: Search sequentially for the first true effectful predicate.

- `STM.fail` -> `Effect.fail`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.failSync` -> `Effect.failSync`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.fiberId` -> `Effect.fiberId`: The operation remains on Effect, but v4 yields the fiber id as a number.

- `STM.filter` -> `Effect.filter`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.filterNot` -> `Effect.filter`: Negate the effectful predicate result and use Effect.filter.

- `STM.filterOrDie` -> `Effect.filterOrFail + Effect.orDie`: Fail with the lazy defect when the predicate rejects, then convert that failure to a defect.

- `STM.filterOrDieMessage` -> `Effect.filterOrFail + Effect.orDie`: Fail with a new Error carrying the message when the predicate rejects, then convert it to a defect.

- `STM.filterOrFail` -> `Effect.filterOrFail`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.firstSuccessOf` -> `Effect.firstSuccessOf`: This only preserves typed-failure fallback. V4 has no equivalent for v3 retry-aware alternatives with journal savepoints.

- `STM.flatMap` -> `Effect.flatMap`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.flatten` -> `Effect.flatten`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.flip` -> `Effect.flip`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.flipWith` -> `Effect.flip(self).pipe(f, Effect.flip)`: Compose the retained Effect.flip operation around the transforming function.

- `STM.fromEither` -> `Effect.fromResult`: V4 replaced Either with Result; migrate the value and use Effect.fromResult.

- `STM.head` -> `Effect.matchEffect`: Map source failures to Option.some, return the first iterable element, and fail with Option.none when empty.

- `STM.if` -> `Effect.suspend or Effect.flatMap`: Select the true or false branch lazily; use flatMap when the condition is effectful.

- `STM.ignore` -> `Effect.ignore`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.interrupt` -> `Effect.interrupt`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.interruptAs` -> `Effect.interrupt`: V4 exposes interruption of the current fiber only; remove the explicit FiberId argument.

- `STM.isFailure` -> `Effect.isFailure`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.isSTM` -> `Effect.isEffect`: STM no longer has a distinct runtime representation; transaction programs are Effects.

- `STM.isSuccess` -> `Effect.isSuccess`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.iterate` -> `Effect.gen loop`: No direct Effect iterate helper remains; carry state in an explicit sequential Effect.gen loop inside Effect.tx.

- `STM.let` -> `Effect.let`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.loop` -> `Effect.gen loop`: No direct Effect loop helper remains; implement the state loop explicitly and collect values unless discard was requested.

- `STM.map` -> `Effect.map`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.mapAttempt` -> `Effect.flatMap(self, (a) => Effect.try(() => f(a)))`: Use Effect.try in flatMap so thrown exceptions remain typed failures rather than defects.

- `STM.mapInputContext` -> `Effect.updateContext`: The context-input mapping operation was renamed on Effect.

- `STM.match` -> `Effect.match`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.matchSTM` -> `Effect.matchEffect`: The Effect-returning match combinator no longer has an STM suffix.

- `STM.mergeAll` -> `Effect.reduce`: Reduce the input Effects sequentially and combine each produced value with the accumulator.

- `STM.none` -> `Effect.matchEffect + Option.match`: Recreate the Option success/error shuffle explicitly; no dedicated helper remains.

- `STM.option` -> `Effect.option`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.orDie` -> `Effect.orDie`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.orDieWith` -> `Effect.mapError + Effect.orDie`: Map the typed error to the desired defect and then use Effect.orDie.

- `STM.orElse` -> `none`: V4 has no exact retry-aware transactional alternative with journal savepoint restoration. Effect.catch is only a failure-only approximation.

- `STM.orElseEither` -> `none`: V4 has no exact retry-aware alternative. For typed failures only, compose Effect.catch and Result tagging manually.

- `STM.orElseFail` -> `Effect.mapError`: Map typed failures to the replacement error; this does not preserve v3 retry fallback semantics.

- `STM.orElseOptional` -> `Effect.catch + Option.match`: Run the fallback for None and re-fail Some errors explicitly.

- `STM.orElseSucceed` -> `Effect.orElseSucceed`: The name remains for typed failures, but v4 does not preserve v3 retry fallback or journal savepoints.

- `STM.orTry` -> `none`: V4 exposes no recoverable retry signal or public transactional savepoint; restructure branch selection before Effect.txRetry.

- `STM.partition` -> `Effect.partition`: STM values are now ordinary Effects. Run the complete transaction with Effect.tx. The tuple is [passes, fails], the reverse of v3 [failures, successes].

- `STM.provideServiceSTM` -> `Effect.provideServiceEffect`: The effectful service provider was renamed on Effect.

- `STM.provideSomeContext` -> `Effect.provideContext or Effect.updateContext`: The dedicated partial-context helper was removed; provide or update the Effect context explicitly.

- `STM.reduce` -> `Effect.reduce`: The combinator remains, but v4 takes the initial state lazily and also passes the element index.

- `STM.reduceAll` -> `Effect.flatMap + Effect.reduce`: Evaluate the initial Effect, then reduce the remaining Effects sequentially.

- `STM.reduceRight` -> `Effect.reduce over a reversed Array`: Materialize and reverse the iterable, then reduce while preserving the old state/element callback order.

- `STM.refineOrDie` -> `Effect.catch + Option.match`: Re-fail Some refined errors and die with the original error for None.

- `STM.refineOrDieWith` -> `Effect.catch + Option.match`: Re-fail Some refined errors and map None to the requested defect.

- `STM.reject` -> `Effect.flatMap + Option.match`: Fail when the partial rejection returns Some; otherwise keep the original success.

- `STM.rejectSTM` -> `Effect.flatMap + Option.match`: Run and fail with the Effect held by Some; otherwise keep the original success.

- `STM.repeatUntil` -> `Effect.repeat(self, { until: predicate })`: The dedicated combinator moved to Effect.repeat options.

- `STM.repeatWhile` -> `Effect.repeat(self, { while: predicate })`: The dedicated combinator moved to Effect.repeat options.

- `STM.replicate` -> `Effect.replicate`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.replicateSTM` -> `Effect.replicateEffect`: Use the effectful replication combinator and keep execution sequential inside Effect.tx.

- `STM.replicateSTMDiscard` -> `Effect.replicateEffect(self, n, { discard: true })`: Use effectful replication with discard enabled and keep execution sequential inside Effect.tx.

- `STM.retry` -> `Effect.txRetry`: Do not use Effect.retry, which retries typed failures by schedule; Effect.txRetry waits for an accessed Tx value to change.

- `STM.retryUntil` -> `Effect.flatMap + Effect.txRetry`: Succeed when the predicate passes; otherwise return Effect.txRetry inside Effect.tx.

- `STM.retryWhile` -> `Effect.flatMap + Effect.txRetry`: Return Effect.txRetry while the predicate passes; otherwise succeed inside Effect.tx.

- `STM.some` -> `Effect.matchEffect + Option.match`: Recreate the Option success/error shuffle explicitly; no dedicated helper remains.

- `STM.succeed` -> `Effect.succeed`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.summarized` -> `Effect.gen`: Run the summary Effect before and after the body, then return the computed summary and body value.

- `STM.sync` -> `Effect.sync`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.tap` -> `Effect.tap`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.tapBoth` -> `Effect.tapError + Effect.tap`: Compose the separate failure and success taps.

- `STM.tapError` -> `Effect.tapError`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.try` -> `Effect.try`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.unless` -> `Effect.when(self, Effect.sync(() => !predicate()))`: V4 Effect.when takes an effectful condition; suspend and negate the old lazy boolean.

- `STM.unlessSTM` -> `Effect.when(self, Effect.map(condition, (b) => !b))`: Negate the effectful condition and use Effect.when.

- `STM.unsome` -> `Effect.matchEffect + Option.match`: Recreate the Option error/success shuffle explicitly; no dedicated helper remains.

- `STM.validateAll` -> `Effect.validate`: The validation combinator was renamed and now returns a NonEmptyArray of errors.

- `STM.validateFirst` -> `Effect.flip + Effect.forEach`: Flip each candidate result, traverse sequentially, then flip the aggregate to preserve all errors when every candidate fails.

- `STM.void` -> `Effect.void`: The combinator keeps its name, but STM values are now ordinary Effects. Run the complete transaction with Effect.tx. V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `STM.when` -> `Effect.when(self, Effect.sync(predicate))`: V4 Effect.when takes an effectful boolean, so suspend the old lazy predicate.

- `STM.whenSTM` -> `Effect.when`: The effectful-condition form is now the only Effect.when form.

- `STM.zipLeft` -> `Effect.zipWith(self, that, (left) => left)`: Use sequential Effect.zipWith and retain the left result.

- `STM.zipRight` -> `Effect.andThen`: Use Effect.andThen for sequential composition that retains the right result.
