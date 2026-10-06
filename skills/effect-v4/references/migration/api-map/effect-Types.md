# v3 → v4 API map: effect/Types

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Types`

- `Types.Concurrency` -> `Types.Concurrency`: Still exported, but v4 removes inherit; replace it with an explicit number or unbounded.

- `Types.Contravariant` -> `Types.Contravariant`: Unchanged contravariant type helper.

- `Types.Covariant` -> `Types.Covariant`: Unchanged covariant type helper.

- `Types.Ctor` -> `new (...args: Array<any>) => T`: The named alias was removed; inline the construct signature or define a local alias.

- `Types.Invariant` -> `Types.Invariant`: Unchanged invariant type helper.

- `Types.MatchRecord` -> `{} extends S ? onTrue : onFalse`: The alias was removed; inline its conditional because Types.VoidIfEmpty has different optional-record behavior.

- `Types.MergeRecord` -> `Types.MergeLeft`: MergeRecord was an alias for the retained left-biased MergeLeft helper.

- `Types.NoExcessProperties` -> `Types.NoExcessProperties`: Retained with equivalent excess-key checking.
