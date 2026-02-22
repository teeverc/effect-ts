# Behavior and Traits (Equivalence, Order, Equal, Hash)

Use this guide when comparing, sorting, deduplicating, or hashing domain values.

## Mental model

- `Equal`/`Hash` provide stable value semantics for hashed collections.
- `Equivalence` describes when two values should be considered the same.
- `Order` provides deterministic comparison/sorting semantics.
- Define these semantics at domain boundaries so behavior is explicit and testable.

## Patterns

- Use value-based equality for domain entities, not object identity.
- Keep comparison semantics close to the domain type they describe.
- Reuse shared equivalence/order definitions in sorting, grouping, and dedupe operations.
- Ensure hash and equality semantics stay consistent.
- Add focused tests for edge cases (case sensitivity, timezone handling, precision rounding).

## Example (shape)

```ts
type User = { readonly id: string; readonly email: string }

// Keep one canonical equality and ordering policy per type.
// Reuse it in sets/maps/sorts instead of ad-hoc inline comparisons.
```

## Pitfalls

- Defining incompatible hash/equality semantics for the same type.
- Scattering inconsistent sort logic across modules.
- Relying on locale/timezone-sensitive string comparison without explicit policy.
- Mixing normalized and raw values in equality checks.

## Docs

- `https://effect.website/docs/behaviour/equivalence/`
- `https://effect.website/docs/behaviour/order/`
- `https://effect.website/docs/trait/equal/`
- `https://effect.website/docs/trait/hash/`
