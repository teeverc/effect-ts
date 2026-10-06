# v3 → v4 API map: effect/Record

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Record`

- `Record.ReadonlyRecord` -> `Record.ReadonlyRecord`: The public type and parameter order are unchanged.

- `Record.ReadonlyRecord.IsFiniteString` -> `Record.ReadonlyRecord.IsFiniteString`: The namespace utility type is unchanged.

- `Record.getEquivalence` -> `Record.makeEquivalence`: Direct rename; pass the value equivalence unchanged.

- `Record.getLefts` -> `Record.getFailures`: Extract Result.Failure values while preserving keys.

- `Record.getRights` -> `Record.getSuccesses`: Extract Result.Success values while preserving keys.

- `Record.modifyOption` -> `Record.modify`: The Option suffix was dropped; missing keys still return Option.none.

- `Record.partition` -> `Record.partition`: Return Result.succeed(value) for a match and Result.fail(value) otherwise; Filter.fromPredicate adapts predicates that only inspect the value. The tuple is now [passes, fails], the reverse of v3 [excluded, satisfying].

- `Record.partitionMap` -> `Record.partition`: Pass a mapper returning Result. The tuple is [passes, fails], the reverse of v3 [failures, successes].

- `Record.replaceOption` -> `Record.replace`: The Option suffix was dropped; missing keys still return Option.none.

- `Record.separate` -> `Record.separate`: The tuple is now [successes, failures], the reverse of v3 [failures, successes].
