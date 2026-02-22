# Behavior and Traits (Equivalence, Order, Equal, Hash) - v4

Use this guide when comparing, sorting, deduplicating, or hashing domain values.

## Mental model

- `Equal`/`Hash` provide stable value semantics for hashed collections.
- `Equivalence` describes when two values should be considered the same.
- `Order` provides deterministic comparison/sorting semantics.
- In v4, `Equal.equals` uses **structural equality by default** for plain objects, arrays, Maps, Sets, Dates, and RegExps.

## v4 Changes: Structural Equality by Default

In v3, `Equal.equals` used reference equality for plain objects and arrays. In v4, structural comparison is the default:

```ts
// v4
import { Equal } from "effect"

Equal.equals({ a: 1 }, { a: 1 })            // true
Equal.equals([1, [2, 3]], [1, [2, 3]])       // true
Equal.equals(new Map([["a", 1]]), new Map([["a", 1]])) // true
Equal.equals(new Set([1, 2]), new Set([1, 2])) // true
Equal.equals(NaN, NaN)                        // true (v3: false)
```

Objects implementing the `Equal` interface still use their custom equality logic.

## Opting Out: `byReference`

If reference equality is needed for a specific object:

```ts
import { Equal } from "effect"

const obj = Equal.byReference({ a: 1 })
Equal.equals(obj, { a: 1 }) // false — reference equality

// Without creating a Proxy (more performant, permanently marks the object):
const obj2 = Equal.byReferenceUnsafe({ a: 1 })
```

## API Changes

| v3                        | v4                        |
| ------------------------- | ------------------------- |
| `Equal.equivalence<T>()`  | `Equal.asEquivalence<T>()`|
| Reference equality default | Structural equality default |
| `Equal.equals(NaN, NaN)` → `false` | `Equal.equals(NaN, NaN)` → `true` |

## Patterns

- Use value-based equality for domain entities, not object identity.
- Keep comparison semantics close to the domain type they describe.
- Reuse shared equivalence/order definitions in sorting, grouping, and dedupe operations.
- Use `Equal.byReference` when reference identity is required.
- Add focused tests for edge cases (case sensitivity, timezone handling, precision rounding).

## Pitfalls

- Expecting reference equality for plain objects (v4 uses structural equality by default).
- Defining incompatible hash/equality semantics for the same type.
- Scattering inconsistent sort logic across modules.
- Relying on locale/timezone-sensitive string comparison without explicit policy.

## Docs

- `https://effect.website/docs/behaviour/equivalence/`
- `https://effect.website/docs/behaviour/order/`
- `https://effect.website/docs/trait/equal/`
- `https://effect.website/docs/trait/hash/`
