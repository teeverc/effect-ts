# v3 → v4 API map: effect/Exit

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Exit`

- `Exit.Exit` -> `Exit.Exit`: Still exported as Exit.Exit\<A, E\> = Exit.Success\<A, E\> | Exit.Failure\<A, E\>; v4 variants share Exit.Exit.Proto and remain Effect values.

- `Exit.ExitUnify` -> `none`: Removed type-level implementation hook; delete direct references. V4 Success and Failure inherit Exit.Exit.Proto, but no exported Exit-specific Unify interface replaces this API.

- `Exit.ExitUnifyIgnore` -> `none`: Removed type-level implementation hook; delete direct references. V4 Success and Failure inherit Exit.Exit.Proto, but no exported Exit-specific Unify interface replaces this API.

- `Exit.Failure` -> `Exit.Failure`: Still exported with \_tag Failure and cause; it now extends Exit.Exit.Proto and no longer exposes the v3 \_op, effect\_instruction\_i0, or Exit-specific Unify fields.

- `Exit.Success` -> `Exit.Success`: Still exported with \_tag Success and value; it now extends Exit.Exit.Proto, defaults E to never, and no longer exposes the v3 \_op, effect\_instruction\_i0, or Exit-specific Unify fields.

- `Exit.all` -> `Exit.asVoidAll + Exit.isSuccess + Option.some / Option.none`: No direct value-collecting v4 equivalent. Materialize the iterable once; return Option.none for empty input, use Exit.asVoidAll to combine every failure, and otherwise collect each Success.value into Exit.succeed and wrap it in Option.some. The parallel option is gone because v4 Cause flattens sequential and parallel composition.

- `Exit.as` -> `Exit.map`: Replace with Exit.map(self, () =\> value); this preserves a failed Exit and returns Exit data rather than a general Effect.

- `Exit.causeOption` -> `Exit.getCause`: Direct rename; still returns Option.some(cause) for Failure and Option.none for Success.

- `Exit.exists` -> `Exit.isSuccess`: No direct v4 combinator; use Exit.isSuccess(self) && predicate(self.value). If callers rely on the refinement overload, retain an explicitly typed wrapper returning self is Exit.Exit\<B\>.

- `Exit.flatMap`: TODO: needs guidance

- `Exit.flatMapEffect` -> `Effect.matchCauseEffectEager`: Use Effect.matchCauseEffectEager(self, { onFailure: cause =\> Effect.succeed(Exit.failCause(cause)), onSuccess: f }). The explicit failure branch is required because v3 preserved an input Failure as a successful outer Effect; plain Effect.flatMap would instead fail the outer Effect.

- `Exit.flatten` -> `Exit.match`: No direct v4 Exit flatten; use Exit.match(self, { onFailure: Exit.failCause, onSuccess: identity }) to return the inner Exit on success and preserve an outer failure as Exit data.

- `Exit.forEachEffect` -> `Effect.flatMapEager + Effect.exit`: Use Effect.exit(Effect.flatMapEager(self, f)). This captures both the original Exit failure and failures from f into the returned Exit while keeping the outer Effect infallible; flatMapEager preserves v3's eager callback selection for an already-resolved Exit.

- `Exit.fromEither` -> `Result.match + Exit.fail / Exit.succeed`: V3 Either is v4 Result. Convert with Result.match(result, { onFailure: Exit.fail, onSuccess: Exit.succeed }); there is no v4 Exit.fromResult constructor.

- `Exit.fromOption` -> `Option.match + Exit.fail / Exit.succeed`: Use Option.match(option, { onNone: () =\> Exit.fail(undefined), onSome: Exit.succeed }) to preserve v3's Exit\<A, void\> contract. Exit.findErrorOption is an accessor and is not a replacement.

- `Exit.getOrElse` -> `Exit.match`: Use Exit.match(self, { onFailure: orElse, onSuccess: identity }); onFailure still receives the full Cause.

- `Exit.isInterrupted` -> `Exit.hasInterrupts`: Direct semantic rename; true for a Failure whose Cause contains at least one Interrupt reason, false for Success.

- `Exit.mapErrorCause` -> `Exit.match + Exit.failCause / Exit.succeed`: No direct v4 combinator. Use Exit.match(self, { onFailure: cause =\> Exit.failCause(f(cause)), onSuccess: Exit.succeed }); f now receives the flattened v4 Cause representation. Cause.map is only equivalent when f merely maps typed errors.

- `Exit.matchEffect` -> `Effect.matchCauseEffectEager`: Direct cause-aware migration because Exit is an Effect in v4. Use the same onFailure/onSuccess handlers; the Eager variant preserves v3's immediate branch selection for resolved Exit values.

- `Exit.zipLeft` -> `Exit.asVoidAll`: Use const checked = Exit.asVoidAll([self, that]); return Exit.isFailure(checked) ? checked : self. This retains the left success and combines dual failures; v4 Cause no longer distinguishes sequential from parallel composition.

- `Exit.zipPar` -> `Exit.asVoidAll + Exit.succeed`: No direct v4 Exit pair combinator. Check Exit.asVoidAll([self, that]); return its Failure, or after narrowing both inputs to Success return Exit.succeed([self.value, that.value]). V4 Cause.combine has no parallel marker.

- `Exit.zipParLeft` -> `Exit.asVoidAll`: Use const checked = Exit.asVoidAll([self, that]); return Exit.isFailure(checked) ? checked : self. This retains the left success and combines dual failures; v4 Cause no longer distinguishes sequential from parallel composition.

- `Exit.zipParRight` -> `Exit.asVoidAll`: Use const checked = Exit.asVoidAll([self, that]); return Exit.isFailure(checked) ? checked : that. This retains the right success and combines dual failures; v4 Cause no longer distinguishes sequential from parallel composition.

- `Exit.zipRight` -> `Exit.asVoidAll`: Use const checked = Exit.asVoidAll([self, that]); return Exit.isFailure(checked) ? checked : that. This retains the right success and combines dual failures; v4 Cause no longer distinguishes sequential from parallel composition.

- `Exit.zipWith` -> `Exit.match`: No direct v4 equivalent. Nested-match both Exits: preserve a lone failure cause, call options.onFailure and Exit.failCause only when both fail, and call Exit.succeed(options.onSuccess(a, b)) when both succeed.
