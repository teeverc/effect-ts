# Behavior and Traits (Equal, Hash, Equivalence, Order)

Use when comparing, sorting, deduplicating, or hashing domain values.

- `Equal` / `Hash`: value semantics used by `Equal.equals`, `HashMap`, `HashSet`, `Array.dedupe`, caches, etc.
- `Equivalence<A>`: `(a, b) => boolean` — a reusable "same?" policy.
- `Order<A>`: `(a, b) => -1 | 0 | 1` — a reusable sorting policy.

Details: `references/migration/equality.md`, `references/migration/api-map/effect-Equal.md`,
`effect-Order.md`, `effect-Equivalence.md`, `effect-Hash.md`.

## Structural equality is the default (v4)

`Equal.equals` compares primitives, arrays, plain objects (enumerable keys, recursively), `Map`s, `Set`s
(order-independent), `Date`s, `RegExp`s, typed arrays, and values implementing `Equal`. Functions compare by
reference unless they implement `Equal`. Circular structures are supported.

```ts
import { Equal } from "effect"

Equal.equals({ a: 1 }, { a: 1 })                         // true  (v3: false)
Equal.equals([1, [2, 3]], [1, [2, 3]])                   // true
Equal.equals(new Map([["a", 1]]), new Map([["a", 1]]))  // true
Equal.equals(new Date("2024-01-01"), new Date("2024-01-01")) // true
Equal.equals(NaN, NaN)                                    // true  (v3: false)
const is5 = Equal.equals(5); is5(5)                       // data-last form
```

`Data.Class`, `Data.TaggedClass`, `Data.TaggedError` and Schema classes compare structurally too:

```ts
import { Data, Equal } from "effect"

class Person extends Data.Class<{ readonly name: string }> {}
Equal.equals(new Person({ name: "Mike" }), new Person({ name: "Mike" })) // true
```

**Gotchas**

- Hashes and object-pair comparison results are cached (WeakMap). **Do not mutate an object after it has been
  hashed or compared** — e.g. after inserting it into a `HashSet`/`HashMap`. For mutable objects, use reference
  semantics (below) or implement `Equal`/`Hash` on an identity.
- If only one operand implements `Equal`, the result is `false`.

## Opting out: reference equality

```ts
import { Equal } from "effect"

const a = Equal.byReference({ a: 1 })   // Proxy; original object unchanged
Equal.equals(a, { a: 1 })               // false

const b = Equal.byReferenceUnsafe({ a: 1 }) // marks the object itself (no proxy, permanent)
```

## Custom equality: implement `Equal` + `Hash`

Contract: if `a[Equal.symbol](b)` is `true`, then `Hash.hash(a) === Hash.hash(b)`.

```ts
import { Equal, Hash } from "effect"

class UserId implements Equal.Equal {
  constructor(readonly id: string) {}
  [Equal.symbol](that: Equal.Equal): boolean {
    return that instanceof UserId && this.id === that.id
  }
  [Hash.symbol](): number {
    return Hash.string(this.id)
  }
}

Equal.equals(new UserId("1"), new UserId("1")) // true
```

`Hash` helpers: `Hash.hash(value)`, `Hash.string`, `Hash.number`, `Hash.combine(h)(self)`, `Hash.array`,
`Hash.structure(obj)`, `Hash.structureKeys(obj, keys)`, `Hash.random(obj)` (identity hash), `Hash.isHash`.
v3 `Hash.cached` was removed — `Hash.hash` caches object hashes automatically.
`Equal.isEqual(u)` checks whether a value implements `Equal`; `Equal.asEquivalence<A>()` wraps `equals`
(v3 `Equal.equivalence`).

## Equivalence

```ts
import { Equivalence, Array } from "effect"

const caseInsensitive = Equivalence.mapInput(Equivalence.String, (s: string) => s.toLowerCase())

interface User { readonly id: number; readonly email: string }
const sameUser = Equivalence.Struct({ id: Equivalence.Number, email: caseInsensitive })
const byId = Equivalence.mapInput(Equivalence.Number, (u: User) => u.id)

declare const users: ReadonlyArray<User>
Array.dedupeWith(users, byId)
```

Instances/constructors: `Equivalence.make`, `strictEqual`, `String`, `Number`, `Boolean`, `BigInt`, `Date`,
`Tuple([...])`, `Array(eq)`, `Struct({...})`, `Record(eq)`, `combine`, `combineAll`, `mapInput`.
Module helpers: `Option.makeEquivalence`, `Result.makeEquivalence`, `Array.makeEquivalence`, `Chunk.makeEquivalence`,
`Redacted.makeEquivalence`, `Struct.makeEquivalence`, `Schema.toEquivalence(schema)`.

## Order

```ts
import { Array, Order } from "effect"

interface User { readonly name: string; readonly age: number }

const byAge = Order.mapInput(Order.Number, (u: User) => u.age)
const byName = Order.mapInput(Order.String, (u: User) => u.name)

declare const users: ReadonlyArray<User>
Array.sort(users, byAge)
Array.sortBy(byAge, Order.flip(byName))(users)    // age asc, then name desc
Order.combine(byAge, byName)
Order.Struct({ age: Order.Number, name: Order.String })
Order.isLessThan(byAge)(a, b); Order.min(byAge)(a, b); Order.clamp(Order.Number)({ minimum: 0, maximum: 10 })
```

Constructors/instances: `Order.make`, `String`, `Number`, `Boolean`, `BigInt`, `Date`, `Tuple([...])`,
`Array(order)`, `Struct({...})`, `flip` (v3 `reverse`), `combine`, `combineAll`, `alwaysEqual` (v3 `empty`),
`mapInput`, `min`, `max`, `clamp`, `isBetween` (v3 `between`), `isLessThan`, `isGreaterThan`,
`isLessThanOrEqualTo`, `isGreaterThanOrEqualTo`. Module orders: `DateTime.Order`, `Duration.Order`,
`BigDecimal.Order`, `ByteSize.Order`, `Option.makeOrder`, `Array.makeOrder`, `Struct.makeOrder`.

## v3 → v4

| v3 | v4 |
| --- | --- |
| reference equality for plain objects/arrays (outside `structuralRegion`) | structural equality by default |
| `Equal.equals(NaN, NaN)` → `false` | `true` |
| `Equal.equivalence<T>()` | `Equal.asEquivalence<T>()` |
| — | `Equal.byReference` / `Equal.byReferenceUnsafe` |
| `Hash.cached` | removed (automatic caching) |
| `Order.number` / `string` / `struct` / `tuple(a, b)` / `array` | `Order.Number` / `String` / `Struct` / `Tuple([a, b])` / `Array` |
| `Order.reverse` / `Order.between` / `Order.lessThan` | `Order.flip` / `Order.isBetween` / `Order.isLessThan` |
| `Equivalence.string` / `struct` / `tuple` | `Equivalence.String` / `Struct` / `Tuple([...])` |
| `X.getEquivalence` / `X.getOrder` | `X.makeEquivalence` / `X.makeOrder` |

## Pitfalls

- Expecting reference equality for plain objects (wrap with `Equal.byReference` if needed).
- Mutating keys of hashed collections.
- Implementing `Equal` without a matching `Hash` (broken `HashMap`/`HashSet` lookups).
- Scattering ad-hoc comparator lambdas instead of sharing named `Order` / `Equivalence` values next to the domain type.
