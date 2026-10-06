# v3 → v4 API map: effect/Chunk

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Chunk`

- `Chunk.Chunk` -> `Chunk.Chunk`: The model remains Chunk.Chunk\<A\>; continue using Chunk constructors rather than depending on its exposed representation fields.

- `Chunk.TypeId` -> `none`: The v4 Chunk brand key is private; no public Chunk.TypeId type or value is exported.

- `Chunk.getEquivalence` -> `Chunk.makeEquivalence`: Direct rename; pass the element Equivalence unchanged.

- `Chunk.modifyOption` -> `Chunk.modify`: The Option suffix was dropped; an out-of-bounds index still returns Option.none.

- `Chunk.partition` -> `Chunk.partition`: Return Result.succeed(value) for a match and Result.fail(value) otherwise; Filter.fromPredicate adapts predicates that only inspect the value. The tuple is now [passes, fails], the reverse of v3 [excluded, satisfying].

- `Chunk.partitionMap` -> `Chunk.partition`: Pass a Result-returning mapper instead of Either. The tuple is [passes, fails], the reverse of v3 [failures, successes].

- `Chunk.removeOption` -> `Chunk.remove`: The closest API now returns the unchanged Chunk out of bounds; use Chunk.get before Chunk.remove to preserve the old Option result.

- `Chunk.replaceOption` -> `Chunk.replace`: The Option suffix was dropped; an out-of-bounds index still returns Option.none.

- `Chunk.separate` -> `Chunk.separate`: The tuple is now [successes, failures], the reverse of v3 [failures, successes].

- `Chunk.unsafeFromArray` -> `Chunk.fromArrayUnsafe`: Direct word-order rename; it still wraps without copying and is unsafe if the source array is mutated.

- `Chunk.unsafeFromNonEmptyArray` -> `Chunk.fromNonEmptyArrayUnsafe`: Direct word-order rename; it still wraps without copying and preserves NonEmptyChunk.

- `Chunk.unsafeGet` -> `Chunk.getUnsafe`: Direct word-order rename; it still throws for an out-of-bounds index.

- `Chunk.unsafeHead` -> `Chunk.headUnsafe`: Direct word-order rename; it still throws on an empty Chunk.

- `Chunk.unsafeLast` -> `Chunk.lastUnsafe`: Direct word-order rename; it still throws on an empty Chunk.
