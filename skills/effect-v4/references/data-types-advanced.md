# Data Types Advanced (DateTime, BigDecimal, HashSet, Redacted)

Use this guide when `Option`/`Either`/`Duration` are not enough and you need stronger domain-level primitives.

## Mental model

- Choose data types that encode invariants directly in types.
- Keep parsing/normalization at boundaries, then operate on typed values inside the domain.
- Prefer deterministic numeric/time behavior over ad-hoc string/number manipulation.

## When to choose what

- `DateTime`: timezone-aware points in time and arithmetic.
- `BigDecimal`: precise decimal math (money/rates), avoid floating-point drift.
- `HashSet`: deduplicated immutable collections with value semantics.
- `Redacted`: secret/sensitive values safe for logs and diagnostics.

## Patterns

- Decode external input to typed values early (schema boundary).
- Keep monetary and rate calculations in `BigDecimal`, convert late.
- Use `HashSet` for identity/uniqueness rules instead of manual array scans.
- Ensure any user/token/password-like value is modeled as `Redacted`.

## Pitfalls

- Using JS `number` for precise currency math.
- Performing timezone math on plain strings.
- Logging sensitive values before redaction.
- Converting typed values back to untyped representations too early.

## Docs

- `https://effect.website/docs/data-types/datetime/`
- `https://effect.website/docs/data-types/bigdecimal/`
- `https://effect.website/docs/data-types/hash-set/`
- `https://effect.website/docs/data-types/redacted/`
