# Data Types Advanced (DateTime, Duration, BigDecimal, HashMap/HashSet, Redacted, ByteSize, Newtype, Brand, Optic)

Use these when plain `number` / `string` / `Date` / arrays lose domain invariants.
Decode external input into these types at the boundary with `Schema` (see `schema.md`), then stay typed.

| Need | Module | Schema codec |
| --- | --- | --- |
| instants, time zones, calendar math | `DateTime` | `Schema.DateTimeUtc`, `DateTimeUtcFromString`, `DateTimeUtcFromMillis`, `DateTimeZoned`, `TimeZone*` |
| time spans | `Duration` | `Schema.Duration`, `DurationFromMillis`, `DurationFromString` |
| exact decimals (money, rates) | `BigDecimal` | `Schema.BigDecimal`, `BigDecimalFromString` |
| value-keyed immutable collections | `HashMap`, `HashSet` | `Schema.HashMap`, `Schema.HashSet` |
| secrets | `Redacted` | `Schema.Redacted(...)`, `Config.Redacted` |
| byte counts | `ByteSize` | `Schema.ByteSize`, `ByteSizeFromString` |
| zero-cost nominal wrapper | `Newtype` | — |
| nominal / refined primitives | `Brand`, `Schema.brand` | `Schema.brand("Id")` |
| immutable focused updates | `Optic` | — |

## DateTime (use instead of `Date` / `Date.now()`)

Bundled examples: `references/examples/07_datetime/10_creating-and-formatting.ts`,
`references/examples/07_datetime/20_time-zones.ts`.

- `DateTime.Utc` and `DateTime.Zoned` (instant + `TimeZone`). Values are immutable.
- Current time: `yield* DateTime.now` (uses `Clock`, so `TestClock` controls it). `DateTime.nowUnsafe()` outside Effect.
- Parse safely: `DateTime.make(input)` → `Option<Utc>` (or `Option<Zoned>` for zoned input);
  input may be `Date`, epoch millis, ISO string, parts object, another `DateTime`.
  `DateTime.makeUnsafe` throws. `DateTime.makeZonedFromString(iso)` → `Option<Zoned>`.
- Zoned construction: `DateTime.makeZoned(input, { timeZone, adjustForTimeZone, disambiguation })` → `Option<Zoned>`,
  `DateTime.makeZonedUnsafe(...)`.
- Math: `DateTime.add(dt, { hours: 2 })`, `subtract`, `addDuration`, `subtractDuration`, `startOf("day")`,
  `endOf`, `nearest`, `distance(a, b)` → signed `Duration` (b − a), `min`, `max`, `clamp`, `between`.
- Compare: `isLessThan`, `isGreaterThan`, `isLessThanOrEqualTo`, `isGreaterThanOrEqualTo`,
  `DateTime.Order`, `DateTime.Equivalence`; `isFuture` / `isPast` (effectful, Clock-based).
- Format: `formatIso`, `formatIsoDate`, `formatIsoDateUtc`, `formatIsoOffset`, `formatIsoZoned`,
  `format(dt, { locale, ...Intl options })`, `formatUtc`, `formatLocal`, `formatIntl(dt, intlFormatter)`.
- Parts: `toParts`, `toPartsUtc`, `getPart(dt, "hour")`, `setParts`, `toEpochMillis`, `toDate`, `toDateUtc`.

```ts
import { DateTime, Effect, Option } from "effect"

Effect.gen(function*() {
  const now = yield* DateTime.now
  const parsed: Option.Option<DateTime.Utc> = DateTime.make("2024-06-15T14:30:00.000Z")
  const endsAt = now.pipe(DateTime.add({ hours: 2 }))
  yield* Effect.log("ISO string:", DateTime.formatIso(endsAt), Option.getOrUndefined(parsed))
})
```

### Time zones and `CurrentTimeZone`

```ts
import { NodeRuntime } from "@effect/platform-node"
import { DateTime, Effect, Option } from "effect"

Effect.gen(function*() {
  const now = yield* DateTime.now
  const auckland = now.pipe(DateTime.setZoneNamedUnsafe("Pacific/Auckland"))         // trusted zone id
  const sydney: Option.Option<DateTime.Zoned> = now.pipe(DateTime.setZoneNamed("Australia/Sydney")) // untrusted id

  // Zoned in the CurrentTimeZone service (requirement: DateTime.CurrentTimeZone)
  const local = yield* DateTime.nowInCurrentZone

  // Interpret a wall-clock string as being in a zone
  const date: DateTime.Zoned = DateTime.makeZonedUnsafe("2026-06-05", {
    timeZone: "Pacific/Auckland",
    adjustForTimeZone: true // otherwise input is treated as UTC
  })
  yield* Effect.log(auckland, Option.getOrUndefined(sydney), local, date)
}).pipe(
  Effect.provide(DateTime.layerCurrentZoneNamed("America/New_York")),
  NodeRuntime.runMain
)
```

- `DateTime.CurrentTimeZone` is a `Context.Service<CurrentTimeZone, TimeZone>` with **no default** — provide it.
- Layers: `layerCurrentZone(zone)`, `layerCurrentZoneNamed(id)` (fails with `IllegalArgumentError`),
  `layerCurrentZoneOffset(ms)`, `layerCurrentZoneLocal`.
- Per-effect: `DateTime.withCurrentZone(zone)`, `withCurrentZoneNamed(id)`, `withCurrentZoneOffset`, `withCurrentZoneLocal`.
- Zone values: `zoneMakeNamed(id)` → `Option`, `zoneMakeNamedUnsafe`, `zoneMakeNamedEffect`, `zoneMakeOffset(ms)`,
  `zoneMakeLocal()`, `zoneFromString`, `zoneToString`; `setZone`, `setZoneOffset`, `setZoneCurrent`, `toUtc`.

v3 → v4: `unsafeMake`→`makeUnsafe`, `unsafeNow`→`nowUnsafe`, `unsafeMakeZoned`→`makeZonedUnsafe`,
`unsafeSetZoneNamed`→`setZoneNamedUnsafe`, `lessThan`→`isLessThan` (etc.), `distanceDuration`→`distance`,
`epochMillis`→`epochMilliseconds`, part names `millis/seconds/minutes/hours`→`millisecond/second/minute/hour`,
`PartsForMath.millis`→`milliseconds`. See `references/migration/api-map/effect-DateTime.md`.

## Duration

```ts
import { Duration } from "effect"

const d = Duration.sum(Duration.seconds(30), Duration.minutes(1))
Duration.toMillis(d)                       // 90000
Duration.isGreaterThan(d, Duration.minutes(1)) // true
Duration.toMillis("5 seconds")             // conversions accept Duration.Input
Duration.format(d)
```

- `Duration.Input` (v3 `DurationInput`): `Duration | number (millis) | bigint (nanos) | [seconds, nanos] |
  "5 seconds" | "Infinity" | "-Infinity" | { hours, minutes, ... }`. Units: nano(s), micro(s), milli(s),
  second(s), minute(s), hour(s), day(s), week(s).
- Constructors: `nanos`, `micros`, `millis`, `seconds`, `minutes`, `hours`, `days`, `weeks`, `zero`,
  `infinity`, `negativeInfinity`. Parse: `fromInput` → `Option` (v3 `decodeUnknown`), `fromInputUnsafe` (v3 `decode`).
- Ops: `sum`, `subtract`, `times`, `divide` (→ `Option`), `min`, `max`, `clamp`, `between`, `isZero`,
  `isFinite`, `isNegative`, `abs`, `negate`, `match`, `parts`, `toSeconds`, `toNanos` (→ `Option`).
- v4 removed `Duration.formatIso` / `fromIso`. Durations hash canonically, so `Duration.seconds(5)` and
  `Duration.nanos(5_000_000_000n)` are the same `HashMap` key.

## BigDecimal

```ts
import { BigDecimal, Option } from "effect"

const price = BigDecimal.fromStringUnsafe("19.99")
const qty = BigDecimal.fromBigInt(3n)
const total = BigDecimal.multiply(price, qty)
const each = BigDecimal.divide(total, BigDecimal.make(7n, 0)) // Option (None on divide by zero)
const rounded = BigDecimal.round(total, { scale: 2, mode: "half-even" })
BigDecimal.format(rounded) // "59.97"
Option.map(each, BigDecimal.format)
```

`make(value: bigint, scale)`, `fromString` / `fromNumber` → `Option`, `*Unsafe` variants throw.
`sum`, `subtract`, `multiply`, `divide`, `remainder`, `negate`, `abs`, `round`/`truncate`/`ceil`/`floor`,
`BigDecimal.Order`, `Equivalence`, `equals`, `toNumberUnsafe`. Keep money in `BigDecimal`, convert late.

## HashMap / HashSet

Immutable, keyed by `Equal`/`Hash`. Since v4 equality is structural by default, plain objects and arrays work as keys
(do not mutate them after use — hashes are cached). See `behavior-traits.md`.

```ts
import { HashMap, HashSet, Option } from "effect"

const m = HashMap.make(["a", 1], ["b", 2]).pipe(HashMap.set("c", 3))
HashMap.get(m, "a")                                   // Option.some(1)
HashMap.modifyAt(m, "a", Option.map((n) => n + 1))
const s = HashSet.make({ id: 1 }, { id: 1 }, { id: 2 }) // size 2 (structural)
HashSet.has(s, { id: 2 })                             // true
```

`MutableHashMap` / `MutableHashSet` exist for local mutable use; `TxHashMap` / `TxHashSet` for `Effect.tx`.

## Redacted

```ts
import { Config, Effect, Redacted } from "effect"

const apiKey = Redacted.make("sk-123", { label: "API_KEY" }) // label is optional (new in v4)
String(apiKey)            // redacted in logs / toString / JSON
Redacted.value(apiKey)    // explicit unwrap at the call site that needs it

const program = Effect.gen(function*() {
  const key = yield* Config.Redacted("API_KEY") // Config<Redacted<string>>
  return Redacted.value(key).length
})
```

`Redacted.isRedacted`, `Redacted.wipeUnsafe` (v3 `unsafeWipe`), `Redacted.makeEquivalence` (v3 `getEquivalence`).
v3's `Secret` module is gone — use `Redacted`.

## ByteSize (new in v4)

Exact non-negative byte counts, a branded `bigint`.

```ts
import { ByteSize, Option } from "effect"

const limit = ByteSize.mebibytes(5)                           // 5 MiB (binary), megabytes = decimal
const parsed = ByteSize.fromString("512 MB")                  // Option<ByteSize>
const ok = Option.exists(parsed, (s) => ByteSize.isLessThanOrEqualTo(s, limit))
ByteSize.format(limit)                                        // human readable
ByteSize.format(limit, { system: "binary", precision: 1 })
```

`bytes`, `kilobytes`…`quettabytes`, `kibibytes`…`yobibytes`, `fromInput`/`fromInputUnsafe`, `fromStringUnsafe`,
`sum`, `subtract` / `times` / `divide` (→ `Option`), `toBigInt`, `toNumber` (→ `Option`), `toUnit`, `Order`, `Equivalence`.

## Newtype (new in v4) and Brand

`Newtype` — compile-time-only nominal wrapper with optics helpers:

```ts
import { Newtype } from "effect"

interface UserId extends Newtype.Newtype<"UserId", number> {}
const UserIdIso = Newtype.makeIso<UserId>()
const id: UserId = UserIdIso.set(1)
Newtype.value(id) // 1
```

Also `Newtype.makeEquivalence`, `makeOrder`, `makeCombiner`, `makeReducer` to reuse carrier instances.

`Brand` — branded primitives (`A & Brand<"Key">`) with optional validation:

```ts
import { Brand, Schema } from "effect"

type Int = number & Brand.Brand<"Int">
const Int = Brand.check<Int>(Schema.isInt())        // validation via Schema checks
Int(1); Int.option(1.5); Int.result(1.5); Int.is(2)

type Even = number & Brand.Brand<"Even">
const Even = Brand.make<Even>((n) => n % 2 === 0 || `Expected ${n} to be even`) // custom filter

type UserId = string & Brand.Brand<"UserId">
const UserId = Brand.nominal<UserId>()              // no runtime checks
const Both = Brand.all(Int, Even)
```

For schemas prefer `Schema.String.pipe(Schema.brand("UserId"))`. **4.0.0 stable**: `Schema.brand` takes a single
identifier and is type-only (brand ids are not stored in the AST); compose distinct brands by applying `brand` /
`Schema.fromBrand` repeatedly. v3 `Brand.refined` → `Brand.make`/`Brand.check`; errors are a single `Brand.BrandError`.
See `references/migration/api-map/effect-Brand.md`.

## Optic (new in v4, brief)

Immutable focused reads/updates: `Iso`, `Lens`, `Prism`, `Optional`, `Traversal`.

```ts
import { Optic } from "effect"

type Person = { readonly name: string; readonly address: { readonly city: string } }
const _city = Optic.id<Person>().key("address").key("city")

const p: Person = { name: "Ada", address: { city: "London" } }
_city.get(p)                                // "London"
_city.replace("Paris", p)                   // new object, original untouched
_city.modify((c) => c.toUpperCase())(p)
```

Builders on optics: `.key`, `.optionalKey`, `.at`, `.tag`, `.pick`, `.omit`, `.refine`, `.check`,
`.notUndefined`, `.compose`, `.forEach`. Constructors: `Optic.makeIso`, `makeLens`, `makePrism`, `makeOptional`;
helpers `Optic.some()`, `Optic.success()`, `Optic.getAll`, `Optic.modifyAll`.

## Pitfalls

- `new Date()` / `Date.now()` in domain code → untestable; use `DateTime.now` / `Clock`.
- Timezone math on strings; forgetting to provide `CurrentTimeZone`.
- JS `number` for money; `BigDecimal.divide` returns `Option` — handle `None`.
- Logging secrets before wrapping them in `Redacted`.
- Mutating objects used as `HashMap`/`HashSet` keys (cached hashes go stale).
