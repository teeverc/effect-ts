# v3 → v4 API map: effect/Take

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Take`

- `Take.Take` -> `Take.Take<A, E, Done>`: v4 Take is the plain union NonEmptyReadonlyArray\<A\> | Exit.Exit\<Done, E\> — no wrapper object or Pipeable: a value batch is a non-empty array, a failure is a failed Exit, and end-of-stream is a successful Exit carrying the Done value (void by default). The module keeps the effect/Take path but exports only the type and toPull.

- `Take.Take.Variance` -> `none`: Variance plumbing removed; v4 Take is a plain union type with no branded interface, so there is nothing to migrate to.

- `Take.TakeTypeId` -> `none`: No brand symbol in v4; discriminate the union with Exit.isExit(take) (Exit branch) vs the non-empty array branch (Array.isReadonlyArrayNonEmpty).

- `Take.chunk` -> `NonEmptyReadonlyArray`: No constructor needed: a value-batch Take is just the non-empty array of values itself (convert a v3 Chunk with Array.fromIterable); empty batches are not representable and must be skipped.

- `Take.dieMessage` -> `Exit.die(new Error(message))`: A defect Take is a died Exit; wrap the message in an Error yourself since there is no dedicated dieMessage helper.

- `Take.done` -> `Take.toPull`: Take.toPull(take) converts a Take into a Pull (Effect succeeding with the batch); end-of-stream surfaces as Cause.Done in the error channel instead of v3's Option.none, and failures keep their cause.

- `Take.fail` -> `Exit.fail`: A failing Take is simply the failed Exit: Exit.fail(error).

- `Take.failCause` -> `Exit.failCause`: A failing Take with a full cause is simply Exit.failCause(cause).

#### `Take.fromEffect`

**Replacement:** `Effect.exit + Exit.isSuccess`

Run the effect with Effect.exit and convert the result: a successful exit value a becomes the single-element batch [a], a failed exit is used directly as the Take.

**Example**

```ts
Effect.map(Effect.exit(effect), (exit) => Exit.isSuccess(exit) ? [exit.value] as const : exit)
```

- `Take.fromExit` -> `Exit.isSuccess(exit) ? [exit.value] : exit`: A success exit becomes the single-element batch [a]; a failure exit is already a valid v4 Take and is used as-is.

#### `Take.fromPull`

**Replacement:** `Effect.matchCause + Pull.doneExitFromCause`

Convert one v4 Pull step into a Take: the success batch is the Take itself, and Pull.doneExitFromCause turns the failure cause into the Exit branch (Cause.Done becomes a successful end Exit, real failures become a failed Exit).

**Example**

```ts
Effect.matchCause(pull, { onSuccess: (arr) => arr, onFailure: Pull.doneExitFromCause })
```

- `Take.isDone` -> `Exit.isExit(take) && Exit.isSuccess(take)`: End-of-stream is the successful-Exit branch of the union.

- `Take.isFailure` -> `Exit.isExit(take) && Exit.isFailure(take)`: A failure Take is the failed-Exit branch of the union.

- `Take.isSuccess` -> `!Exit.isExit(take)`: A value batch is the non-Exit branch; use Array.isReadonlyArrayNonEmpty(take) when a positive refinement to NonEmptyReadonlyArray\<A\> is needed.

- `Take.make` -> `none`: No wrapper constructor: build the union value directly — a non-empty array for values, Exit.fail/Exit.failCause for errors, Exit.succeed(done) (or Exit.void) for end-of-stream; the v3 Exit\<Chunk\<A\>, Option\<E\>\> encoding is gone.

- `Take.map` -> `Exit.isExit(take) ? take : Array.map(take, f)`: Only the value batch is mapped; effect's Array.map preserves the NonEmptyReadonlyArray type, and Exit branches (failure/end) pass through unchanged.

#### `Take.match`

**Replacement:** `Exit.isExit + Exit.match`

Branch on the union: the array branch is v3's onSuccess(chunk), and Exit.match splits the Exit branch into onFailure(cause) and end-of-stream (v3 onEnd, success value = Done).

**Example**

```ts
// v3: Take.match(take, { onEnd, onFailure, onSuccess })
Exit.isExit(take)
  ? Exit.match(take, { onSuccess: () => onEnd(), onFailure: (cause) => onFailure(cause) })
  : onSuccess(take)

```

- `Take.matchEffect` -> `Pull.matchEffect(Take.toPull(take), { onSuccess, onFailure, onDone })`: Convert with Take.toPull and fold with Pull.matchEffect: onSuccess receives the batch (v3 onSuccess), onFailure the cause, onDone the completion value (v3 onEnd); alternatively branch manually with Exit.isExit as for match.

- `Take.of` -> `[value]`: A single-value Take is just the one-element non-empty array literal.

- `Take.tap` -> `Exit.isExit(take) ? Exit.asVoid(take) : Effect.asVoid(f(take))`: Peek at the value batch with f; Exit branches pass through as effects (a failed Exit re-propagates its cause, an end Exit becomes a void success), matching v3 tap semantics.
