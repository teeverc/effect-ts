# v3 → v4 API map: effect/Either

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Either`

- `Either.Do` -> `Result.Do`: The empty successful do-notation value moved to Result.

- `Either.Either` -> `Result.Result`: Either\<A, E\> became Result\<A, E\>; Right and Left became Success and Failure.

- `Either.Either.Left` -> `Result.Result.Failure`: Use the Result namespace extractor for the failure variant.

- `Either.Either.Right` -> `Result.Result.Success`: Use the Result namespace extractor for the success variant.

- `Either.EitherTypeLambda` -> `Result.ResultTypeLambda`: Moved and renamed with Result.

- `Either.EitherUnify` -> `Result.ResultUnify`: Moved and renamed with Result.

- `Either.EitherUnifyIgnore` -> `Result.ResultUnifyIgnore`: Moved and renamed with Result.

- `Either.Left` -> `Result.Failure`: Left became Failure; .left became .failure.

- `Either.Right` -> `Result.Success`: Right became Success; .right became .success.

- `Either.TypeId` -> `none`: Result keeps its brand private and exports no public TypeId.

- `Either.all` -> `Result.all`: Either moved to Result; collection behavior is retained with Failure and Success terminology.

- `Either.ap` -> `Result.flatMap`: Use Result.flatMap(self, (f) =\> Result.map(that, f)); v4 has no Result.ap.

- `Either.bind` -> `Result.bind`: The do-notation combinator moved to Result.

- `Either.bindTo` -> `Result.bindTo`: The do-notation combinator moved to Result.

- `Either.filterOrLeft` -> `Result.filterOrFail`: Left is now Failure, so the predicate combinator is filterOrFail.

- `Either.flatMap`: TODO: needs guidance

- `Either.flip` -> `Result.flip`: The channel-swapping combinator moved to Result.

- `Either.fromNullable` -> `Result.fromNullishOr`: Renamed with v4 nullish-or terminology.

- `Either.getEquivalence` -> `Result.makeEquivalence`: Pass success and failure equivalences positionally instead of a right and left object.

- `Either.getLeft` -> `Result.getFailure`: Extract the Result failure as an Option.

- `Either.getOrElse` -> `Result.getOrElse`: Moved unchanged to Result.

- `Either.getOrThrow` -> `Result.getOrThrow`: V4 throws the raw Failure value; use getOrThrowWith when a custom Error is required.

- `Either.getOrThrowWith` -> `Result.getOrThrowWith`: Moved to Result; the callback receives the Failure value.

- `Either.getOrUndefined` -> `Result.getOrUndefined`: Moved unchanged to Result.

- `Either.getRight` -> `Result.getSuccess`: Extract the Result success as an Option.

- `Either.isEither` -> `Result.isResult`: Renamed with the data type.

- `Either.isLeft` -> `Result.isFailure`: Left is now the Failure variant.

- `Either.isRight` -> `Result.isSuccess`: Right is now the Success variant.

- `Either.left` -> `Result.fail`: Construct a Failure with Result.fail.

- `Either.let` -> `Result.let`: The do-notation combinator moved to Result.

- `Either.map` -> `Result.map`: Map now transforms the Success channel.

- `Either.mapLeft` -> `Result.mapError`: Left mapping became failure-channel error mapping.

- `Either.match` -> `Result.match`: Rename handlers from onLeft and onRight to onFailure and onSuccess.

- `Either.right` -> `Result.succeed`: Construct a Success with Result.succeed.

- `Either.try` -> `Result.try`: The synchronous throwable constructor moved to Result.

- `Either.void` -> `Result.void`: Use the prebuilt successful Result\<void\>.
