# v3 → v4 API map: effect/Cause

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Cause`

- `Cause.Cause` -> `Cause.Cause`: The name remains, but v4 Cause\<E\> is a wrapper with readonly reasons: ReadonlyArray\<Cause.Reason\<E\>\>, not the v3 Empty/Fail/Die/Interrupt/Sequential/Parallel tree.

- `Cause.Cause.Variance` -> `none`: The public variance helper was removed. Cause.Cause is directly branded by Cause.TypeId; application code should not reproduce the old variance member.

- `Cause.CauseReducer` -> `cause.reasons.reduce`: The six-case tree reducer type was removed with Empty, Sequential, and Parallel. Reduce the flat Reason array and switch on Fail, Die, or Interrupt instead.

- `Cause.CauseTypeId` -> `Cause.TypeId`: The brand export is Cause.TypeId, a literal-string const. Use typeof Cause.TypeId in type positions; the v3 unique-symbol CauseTypeId alias is gone.

- `Cause.Die` -> `Cause.Die`: The name remains, but Cause.Die is now a Reason stored in cause.reasons, not a Cause variant. Construct a standalone reason with Cause.makeDieReason or a cause with Cause.die.

- `Cause.Empty` -> `Cause.empty`: The Empty subtype and \_tag were removed. Empty is Cause.empty, represented by cause.reasons.length === 0.

- `Cause.ExceededCapacityException` -> `Cause.ExceededCapacityError`: Rename the class/type and update the discriminant from ExceededCapacityException to ExceededCapacityError.

- `Cause.ExceededCapacityExceptionTypeId` -> `Cause.ExceededCapacityErrorTypeId`: Rename the brand; v4 exports a literal-string const, so use typeof Cause.ExceededCapacityErrorTypeId in type positions.

- `Cause.Fail` -> `Cause.Fail`: The name remains, but Cause.Fail\<E\> is now a Reason stored in cause.reasons, not a Cause variant. Construct a standalone reason with Cause.makeFailReason or a cause with Cause.fail.

- `Cause.IllegalArgumentException` -> `Cause.IllegalArgumentError`: Rename the class/type and update the discriminant from IllegalArgumentException to IllegalArgumentError.

- `Cause.IllegalArgumentExceptionTypeId` -> `Cause.IllegalArgumentErrorTypeId`: Rename the brand; v4 exports a literal-string const, so use typeof Cause.IllegalArgumentErrorTypeId in type positions.

- `Cause.Interrupt` -> `Cause.Interrupt`: The name remains, but it is now a Reason in cause.reasons rather than a Cause variant, and fiberId changed from FiberId.FiberId to number | undefined. Use Cause.makeInterruptReason or Cause.interrupt.

- `Cause.InterruptedException` -> `none`: The public exception class was removed. Represent cancellation with Cause.interrupt; Cause.prettyErrors creates an ordinary Error named InterruptError for interrupt-only rendering, but no class is exported.

- `Cause.InterruptedExceptionTypeId` -> `none`: Removed with InterruptedException; v4 exports no interruption-error brand. Inspect the Cause with Cause.hasInterrupts or Cause.hasInterruptsOnly instead.

- `Cause.InvalidPubSubCapacityException` -> `Error`: The dedicated public type was removed. Current v4 PubSub capacity validation throws a standard global Error with the capacity message.

- `Cause.InvalidPubSubCapacityExceptionTypeId` -> `none`: Removed with InvalidPubSubCapacityException; the standard Error now thrown by PubSub has no Effect-specific brand.

- `Cause.NoSuchElementException` -> `Cause.NoSuchElementError`: Rename the class/type and update the discriminant from NoSuchElementException to NoSuchElementError.

- `Cause.NoSuchElementExceptionTypeId` -> `Cause.NoSuchElementErrorTypeId`: Rename the brand; v4 exports a literal-string const, so use typeof Cause.NoSuchElementErrorTypeId in type positions.

- `Cause.Parallel` -> `none`: Parallel cause nodes were removed; v4 stores all reasons in one flat array and does not retain composition kind.

- `Cause.PrettyError` -> `Error`: Cause.prettyErrors now returns Array\<globalThis.Error\>. The dedicated span field is gone; tracing information is incorporated from Reason annotations into rendered stacks.

- `Cause.RuntimeException` -> `Error`: The dedicated class was removed and v4 uses global Error for generic defects. Use Data.Error or Data.TaggedError instead when a yieldable typed error is required.

- `Cause.RuntimeExceptionTypeId` -> `none`: Removed with RuntimeException. Define and guard a custom Data.Error/Data.TaggedError type if nominal branding is required.

- `Cause.Sequential` -> `none`: Sequential cause nodes were removed; v4 stores all reasons in one flat array and does not retain composition kind.

- `Cause.TimeoutException` -> `Cause.TimeoutError`: Rename the class/type and update the discriminant from TimeoutException to TimeoutError.

- `Cause.TimeoutExceptionTypeId` -> `Cause.TimeoutErrorTypeId`: Rename the brand; v4 exports a literal-string const, so use typeof Cause.TimeoutErrorTypeId in type positions.

- `Cause.UnknownException` -> `Cause.UnknownError`: Rename the class/type and discriminant. The original unknown value is now exposed through the standard Error.cause property, not v3's .error field.

- `Cause.UnknownExceptionTypeId` -> `Cause.UnknownErrorTypeId`: Rename the brand; v4 exports a literal-string const, so use typeof Cause.UnknownErrorTypeId in type positions.

- `Cause.YieldableError` -> `Cause.YieldableError / Data.Error`: Cause.YieldableError remains as the interface/type, but its public constructor value was removed. Extend Data.Error for an untagged yieldable error or Data.TaggedError for a tagged one.

- `Cause.andThen` -> `Cause.fromReasons(self.reasons.flatMap(...))`: No direct v4 combinator. For each Fail reason, splice either f(reason.error).reasons or the constant cause's reasons; retain Die and Interrupt reasons, then rebuild with Cause.fromReasons.

- `Cause.as` -> `Cause.map(self, () => error)`: Use Cause.map with a constant function; only Fail errors change and Die/Interrupt reasons remain.

- `Cause.contains` -> `Equal.equals(Cause.combine(self, that), self)`: There are no subtrees in v4. This tests whether all reasons from that are already present in self under v4 reason equality; use Equal.equals(self, that) when only whole-cause equality is intended.

- `Cause.defects` -> `self.reasons.filter(Cause.isDieReason).map((reason) => reason.defect)`: Collect defect values from the flat Reason array. The result is a standard array rather than v3 Chunk.

- `Cause.dieOption` -> `Cause.findDefect`: Cause.findDefect returns Result.Result\<unknown, Cause.Cause\<E\>\>, not Option. Match the Result or convert it to Option when the old return shape is required.

- `Cause.failureOption` -> `Cause.findErrorOption`: Direct Option-based replacement for extracting the first typed Fail error value.

- `Cause.failureOrCause` -> `Cause.findError`: Use the v4 Result-based split: success is the first E and failure is the original Cause\<never\> when no Fail reason exists.

- `Cause.failures` -> `self.reasons.filter(Cause.isFailReason).map((reason) => reason.error)`: Collect typed error values from the flat Reason array. The result is a standard array rather than v3 Chunk.

- `Cause.filter` -> `Cause.fromReasons(self.reasons.filter(...))`: No exact tree-level equivalent: v3 predicates selected recursive child causes. Rewrite the predicate for Cause.Reason values, filter cause.reasons, and rebuild with Cause.fromReasons.

- `Cause.find` -> `Option.firstSomeOf(self.reasons.map(...))`: No recursive nodes remain. Apply the partial function to Reason values and take the first Some, or use Cause.findFail/findError/findDie/findDefect/findInterrupt for standard searches.

- `Cause.flatMap` -> `Cause.fromReasons(self.reasons.flatMap((reason) => Cause.isFailReason(reason) ? f(reason.error).reasons : [reason]))`: No direct v4 export. Flat-map only Fail reasons into replacement causes, preserve Die/Interrupt reasons, and rebuild from the resulting Reason array.

- `Cause.flatten` -> `Cause.fromReasons(self.reasons.flatMap((reason) => Cause.isFailReason(reason) ? reason.error.reasons : [reason]))`: No direct v4 export. For Cause\<Cause\<E\>\>, splice each Fail reason's nested cause.reasons and retain Die/Interrupt reasons.

- `Cause.flipCauseOption` -> `Cause.fromReasons + Option`: Rewrite over reasons: drop Fail(None), replace Fail(Some(e)) with Cause.makeFailReason(e), retain Die/Interrupt, then return None only when a non-empty input loses every reason; preserve Some(Cause.empty) for an empty input.

- `Cause.interruptOption` -> `Cause.findInterrupt`: The replacement returns Result.Result\<Cause.Interrupt, Cause.Cause\<E\>\> rather than Option\<FiberId\>; on success read reason.fiberId, now number | undefined.

- `Cause.isDie` -> `Cause.hasDies`: Use the v4 cause-level predicate for the presence of at least one Die reason.

- `Cause.isDieType` -> `Cause.isDieReason`: Apply this guard to an entry of cause.reasons; Cause itself is no longer a Die union variant.

- `Cause.isEmpty` -> `self.reasons.length === 0`: V4 represents an empty cause with an empty reasons array and exports no isEmpty function.

- `Cause.isEmptyType` -> `self.reasons.length === 0`: The check remains possible, but there is no Empty subtype to narrow to because v4 Cause is not a variant union.

- `Cause.isExceededCapacityException` -> `Cause.isExceededCapacityError`: Rename the guard along with ExceededCapacityError.

- `Cause.isFailType` -> `Cause.isFailReason`: Apply this guard to an entry of cause.reasons; Cause itself is no longer a Fail union variant.

- `Cause.isFailure` -> `Cause.hasFails`: Use the v4 cause-level predicate for the presence of at least one Fail reason.

- `Cause.isIllegalArgumentException` -> `Cause.isIllegalArgumentError`: Rename the guard along with IllegalArgumentError.

- `Cause.isInterruptType` -> `Cause.isInterruptReason`: Apply this guard to an entry of cause.reasons; Cause itself is no longer an Interrupt union variant.

- `Cause.isInterrupted` -> `Cause.hasInterrupts`: Use the v4 cause-level predicate for the presence of at least one Interrupt reason.

- `Cause.isInterruptedException` -> `none`: No v4 InterruptError class or unknown-value guard is exported. When the Cause is available, test Cause.hasInterruptsOnly before squashing or rendering it.

- `Cause.isInterruptedOnly` -> `Cause.hasInterruptsOnly`: Direct cause-level rename; it is false for Cause.empty and true only when at least one reason exists and every reason is Interrupt.

- `Cause.isNoSuchElementException` -> `Cause.isNoSuchElementError`: Rename the guard along with NoSuchElementError.

- `Cause.isParallelType` -> `none`: Parallel cause nodes were removed; v4 stores all reasons in one flat array and does not retain composition kind.

- `Cause.isRuntimeException` -> `none`: RuntimeException and its brand were removed. Use instanceof Error for generic errors or define a Data.Error/Data.TaggedError class with its own guard when nominal recognition is required.

- `Cause.isSequentialType` -> `none`: Sequential cause nodes were removed; v4 stores all reasons in one flat array and does not retain composition kind.

- `Cause.isTimeoutException` -> `Cause.isTimeoutError`: Rename the guard along with TimeoutError.

- `Cause.isUnknownException` -> `Cause.isUnknownError`: Rename the guard along with UnknownError.

- `Cause.keepDefects` -> `Cause.fromReasons(self.reasons.filter(Cause.isDieReason))`: Keep every Die reason, not merely the first defect. Return Option.none when the filtered array is empty and Option.some of the rebuilt cause otherwise; Cause.findDefect alone is not behaviorally equivalent.

- `Cause.linearize` -> `self.reasons`: No direct replacement: v4 discarded sequential/parallel structure, so there are no parallel branches to linearize. Rewrite the consumer to process the flat Reason array.

- `Cause.originalError` -> `Function.identity`: V3 used this to unwrap span-capture proxies. V4 stores tracing data on Reason.annotations and no longer proxies errors, so the input is already the original value.

- `Cause.parallel` -> `Cause.combine`: Combine the two flat reason arrays; v4 intentionally no longer records whether composition was parallel or sequential.

- `Cause.reduce` -> `self.reasons.reduce`: Reduce the flat Reason array directly. The callback now sees only Fail, Die, and Interrupt reasons, never Empty or composition nodes.

- `Cause.reduceWithContext` -> `self.reasons.reduce`: Capture the context in the reducer closure and reduce the flat Reason array; sequentialCase and parallelCase have no v4 analogue.

- `Cause.sequential` -> `Cause.combine`: Combine the two flat reason arrays; v4 intentionally no longer records whether composition was parallel or sequential.

- `Cause.size` -> `self.reasons.length`: The v3 node count becomes the number of flat reasons in v4.

- `Cause.squashWith` -> `Result.match(Cause.findError(self), { onSuccess: f, onFailure: Cause.squash })`: Apply f only to the first typed Fail error; if no Fail exists, squash the returned Cause\<never\>. This preserves v3's priority and avoids evaluating f for later Fail reasons.

- `Cause.stripFailures` -> `Cause.fromReasons(self.reasons.filter((reason) => !Cause.isFailReason(reason)))`: Remove Fail reasons and retain Die plus Interrupt reasons, then rebuild the cause. The v3 prose saying interrupts were removed did not match its implementation.

- `Cause.stripSomeDefects` -> `Cause.fromReasons + Option`: Filter out each Die reason for which pf(reason.defect) is Some, retain all other reasons, and rebuild. Return None only when a non-empty input loses every reason; preserve Some(Cause.empty) for empty input.
