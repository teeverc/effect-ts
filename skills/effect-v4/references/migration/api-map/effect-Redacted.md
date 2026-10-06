# v3 → v4 API map: effect/Redacted

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Redacted`

- `Redacted.Redacted` -> `Redacted.Redacted`: The sensitive-value wrapper remains and now optionally carries a label.

- `Redacted.Redacted.Variance` -> `Redacted.Redacted.Variance`: The type-level variance member remains.

- `Redacted.RedactedTypeId` -> `Redacted.isRedacted`: The marker is private in v4; use the public guard for runtime narrowing.

- `Redacted.getEquivalence` -> `Redacted.makeEquivalence`: Renamed to the v4 make-prefix convention.

- `Redacted.unsafeWipe` -> `Redacted.wipeUnsafe`: Renamed to use the v4 Unsafe suffix convention.
