# v3 → v4 API map: effect/Match

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Match`

- `Match.Matcher` -> `Match.Matcher`: The type is retained, but its fifth argument is now a flavor marker (ValueFlavor for Match.value and never for Match.type or Match.fn) rather than the provided value; an optional seventh Args tuple tracks Match.fn selector arguments. Prefer inference from Match.type, Match.value, or Match.fn and update hand-written Matcher annotations.

- `Match.MatcherTypeId` -> `none`: The public matcher brand was internalized. Obtain matchers from Match.type or Match.value and use their public \_tag when discrimination is required.

- `Match.Not` -> `Match.Not`: The case type is retained. Its evaluate method now receives any Match.fn selector arguments after the selected input; update custom case implementations that consume those arguments.

- `Match.SafeRefinementId` -> `none`: The public safe-refinement brand was internalized. Use Predicate.Refinement, Predicate.Predicate, or a built-in Match refinement instead of constructing the brand.

- `Match.TypeMatcher` -> `Match.TypeMatcher`: The public type is retained, but its brand is private; create values with Match.type rather than implementing the interface.

- `Match.Types` -> `Match.Types`: The public type-level namespace is retained with no call-site migration.

- `Match.Types.ExtractAndNarrow` -> `Match.Types.ExtractAndNarrow`: The type-only matching helper is retained unchanged.

- `Match.Types.MaybeReplace` -> `Match.Types.MaybeReplace`: The type-only matching helper is retained unchanged.

- `Match.Types.NonFailKeys` -> `Match.Types.NonFailKeys`: The type-only matching helper is retained unchanged.

- `Match.Types.PForNotMatch` -> `Match.Types.PForNotMatch`: The type-only matching helper is retained unchanged.

- `Match.Types.ResolvePred` -> `Match.Types.ResolvePred`: The type-only matching helper is retained unchanged.

- `Match.Types.SafeRefinementR` -> `Match.Types.SafeRefinementR`: The type-only matching helper is retained unchanged.

- `Match.Types.ToInvertedRefinement` -> `Match.Types.ToInvertedRefinement`: The type-only matching helper is retained unchanged.

- `Match.Types.ToSafeRefinement` -> `Match.Types.ToSafeRefinement`: The type-only matching helper is retained unchanged.

- `Match.ValueMatcher` -> `Match.ValueMatcher`: The type is retained, but value now uses Result instead of Either and the brand is private; create values with Match.value.

- `Match.When` -> `Match.When`: The case type is retained. Its evaluate method now receives any Match.fn selector arguments after the selected input; update custom case implementations that consume those arguments.

- `Match.either` -> `Match.result`: Renamed finalizer with a container change: matched Right and unmatched Left become Result.Success and Result.Failure.
