# v3 → v4 API map: effect/MergeDecision

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MergeDecision`

- `MergeDecision.Await` -> `Channel.merge`: Replace Channel.mergeWith decision callbacks with Channel.merge and haltStrategy. V4 has no MergeDecision values; custom effectful exit folding must be restructured.

- `MergeDecision.AwaitConst` -> `Channel.merge`: Replace Channel.mergeWith decision callbacks with Channel.merge and haltStrategy. V4 has no MergeDecision values; custom effectful exit folding must be restructured.

- `MergeDecision.Done` -> `Channel.merge`: Replace Channel.mergeWith decision callbacks with Channel.merge and haltStrategy. V4 has no MergeDecision values; custom effectful exit folding must be restructured.

- `MergeDecision.MergeDecision` -> `Channel.merge`: Replace Channel.mergeWith decision callbacks with Channel.merge and haltStrategy. V4 has no MergeDecision values; custom effectful exit folding must be restructured.

- `MergeDecision.MergeDecision.Variance` -> `Channel.merge`: Replace Channel.mergeWith decision callbacks with Channel.merge and haltStrategy. V4 has no MergeDecision values; custom effectful exit folding must be restructured.

- `MergeDecision.MergeDecisionTypeId` -> `Channel.merge`: Replace Channel.mergeWith decision callbacks with Channel.merge and haltStrategy. V4 has no MergeDecision values; custom effectful exit folding must be restructured.

- `MergeDecision.isMergeDecision` -> `Channel.merge`: Replace Channel.mergeWith decision callbacks with Channel.merge and haltStrategy. V4 has no MergeDecision values; custom effectful exit folding must be restructured.

- `MergeDecision.match` -> `Channel.merge`: Replace Channel.mergeWith decision callbacks with Channel.merge and haltStrategy. V4 has no MergeDecision values; custom effectful exit folding must be restructured.
