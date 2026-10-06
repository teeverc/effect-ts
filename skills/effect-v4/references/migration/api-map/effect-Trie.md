# v3 → v4 API map: effect/Trie

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Trie`

- `Trie.TypeId` -> `none`: The Trie brand is private and there is no public Trie runtime guard; use Trie.Trie\<V\> in type positions.

- `Trie.unsafeGet` -> `Trie.getUnsafe`: Direct word-order rename; it still throws for a missing key.
