# v3 → v4 API map: effect/TRandom

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TRandom`

- `TRandom.TRandom` -> `none`: The transactional random service was deliberately removed; use Random outside retried transactions where possible.

- `TRandom.TRandomTypeId` -> `none`: TRandom and its public type id were removed; v4 has no TxRandom module.

- `TRandom.Tag` -> `Random.Random`: Use the v4 Random Context.Reference; the transactional random service was removed.

- `TRandom.next` -> `Random.next`: TxRandom was removed. Random.next is an ordinary Effect and may be re-executed if used inside a retried transaction.

- `TRandom.nextBoolean` -> `Random.nextBoolean`: TxRandom was removed. This ordinary Effect is not rollback-safe under transaction retry.

- `TRandom.nextInt` -> `Random.nextInt`: TxRandom was removed. This ordinary Effect is not rollback-safe under transaction retry.

- `TRandom.nextIntBetween` -> `Random.nextIntBetween(low, high, { halfOpen: true })`: TxRandom was removed; request half-open bounds explicitly to preserve the v3 range behavior.

- `TRandom.nextRange` -> `Random.nextBetween`: The operation was renamed and is no longer backed by rollback-safe transactional random state.

- `TRandom.shuffle` -> `Random.shuffle`: TxRandom was removed. This ordinary Effect is not rollback-safe under transaction retry.
