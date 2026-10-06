# v3 → v4 API map: effect/StreamHaltStrategy

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/StreamHaltStrategy`

- `StreamHaltStrategy.Both` -> `"both"`: The tagged constructor is replaced by the plain string literal "both" passed directly to haltStrategy options.

- `StreamHaltStrategy.Either` -> `"either"`: The tagged constructor is replaced by the plain string literal "either" passed directly to haltStrategy options.

- `StreamHaltStrategy.HaltStrategy` -> `Stream.HaltStrategy`: The StreamHaltStrategy module is gone; v4 HaltStrategy is the string-literal union "left" | "right" | "both" | "either" (defined in Channel, re-exported as Stream.HaltStrategy) instead of tagged objects.

- `StreamHaltStrategy.HaltStrategyInput` -> `Stream.HaltStrategy`: The Input widening (tagged object OR string) is obsolete; v4 only ever uses the string literals, so haltStrategy options take Stream.HaltStrategy directly.

- `StreamHaltStrategy.Left` -> `"left"`: The tagged constructor is replaced by the plain string literal "left" passed directly to haltStrategy options.

- `StreamHaltStrategy.Right` -> `"right"`: The tagged constructor is replaced by the plain string literal "right" passed directly to haltStrategy options.

- `StreamHaltStrategy.fromInput` -> `none`: Remove the call; there is no conversion step in v4 because strategies already are the string literals, so pass the value through unchanged.

- `StreamHaltStrategy.isBoth` -> `strategy === "both"`: Refinements on the tagged union become plain string comparison against the literal.

- `StreamHaltStrategy.isEither` -> `strategy === "either"`: Refinements on the tagged union become plain string comparison against the literal.

- `StreamHaltStrategy.isLeft` -> `strategy === "left"`: Refinements on the tagged union become plain string comparison against the literal.

- `StreamHaltStrategy.isRight` -> `strategy === "right"`: Refinements on the tagged union become plain string comparison against the literal.

#### `StreamHaltStrategy.match`

**Replacement:** `switch (strategy)`

Fold over the strategy with an ordinary switch (or ternary chain) on the string literal; TypeScript exhaustiveness-checks the four cases.

**Example**

```ts
// v3: HaltStrategy.match(s, { onLeft, onRight, onBoth, onEither })
switch (strategy) {
  case "left": return onLeft()
  case "right": return onRight()
  case "both": return onBoth()
  case "either": return onEither()
}

```
