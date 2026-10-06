# v3 → v4 API map: effect/FastCheck

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/FastCheck`

- `FastCheck.Arbitrary`: TODO: needs guidance

- `FastCheck.ArrayConstraints`: TODO: needs guidance

- `FastCheck.AsyncCommand`: TODO: needs guidance

- `FastCheck.AsyncPropertyHookFunction`: TODO: needs guidance

- `FastCheck.BigIntArrayConstraints`: TODO: needs guidance

- `FastCheck.BigIntConstraints`: TODO: needs guidance

- `FastCheck.BigUintConstraints` -> `FastCheck.BigIntConstraints`: Depend on fast-check and import it directly. Unsigned bigint constraints were consolidated into BigIntConstraints with min: 0n.

- `FastCheck.CloneValue`: TODO: needs guidance

- `FastCheck.Command`: TODO: needs guidance

- `FastCheck.CommandsContraints`: TODO: needs guidance

- `FastCheck.ContextValue`: TODO: needs guidance

- `FastCheck.DateConstraints`: TODO: needs guidance

- `FastCheck.DepthContext`: TODO: needs guidance

- `FastCheck.DepthIdentifier`: TODO: needs guidance

- `FastCheck.DepthSize`: TODO: needs guidance

- `FastCheck.DictionaryConstraints`: TODO: needs guidance

- `FastCheck.DomainConstraints`: TODO: needs guidance

- `FastCheck.DoubleConstraints`: TODO: needs guidance

- `FastCheck.EmailAddressConstraints`: TODO: needs guidance

- `FastCheck.ExecutionStatus`: TODO: needs guidance

- `FastCheck.ExecutionTree`: TODO: needs guidance

- `FastCheck.FalsyContraints`: TODO: needs guidance

- `FastCheck.FalsyValue`: TODO: needs guidance

- `FastCheck.Float32ArrayConstraints`: TODO: needs guidance

- `FastCheck.Float64ArrayConstraints`: TODO: needs guidance

- `FastCheck.FloatConstraints`: TODO: needs guidance

- `FastCheck.GeneratorValue`: TODO: needs guidance

- `FastCheck.GlobalAsyncPropertyHookFunction`: TODO: needs guidance

- `FastCheck.GlobalParameters`: TODO: needs guidance

- `FastCheck.GlobalPropertyHookFunction`: TODO: needs guidance

- `FastCheck.IAsyncProperty`: TODO: needs guidance

- `FastCheck.IAsyncPropertyWithHooks`: TODO: needs guidance

- `FastCheck.ICommand`: TODO: needs guidance

- `FastCheck.IProperty`: TODO: needs guidance

- `FastCheck.IPropertyWithHooks`: TODO: needs guidance

- `FastCheck.IRawProperty`: TODO: needs guidance

- `FastCheck.IntArrayConstraints`: TODO: needs guidance

- `FastCheck.IntegerConstraints`: TODO: needs guidance

- `FastCheck.JsonSharedConstraints`: TODO: needs guidance

- `FastCheck.LetrecLooselyTypedBuilder`: TODO: needs guidance

- `FastCheck.LetrecLooselyTypedTie`: TODO: needs guidance

- `FastCheck.LetrecTypedBuilder`: TODO: needs guidance

- `FastCheck.LetrecTypedTie`: TODO: needs guidance

- `FastCheck.LetrecValue`: TODO: needs guidance

- `FastCheck.LoremConstraints`: TODO: needs guidance

- `FastCheck.MaybeWeightedArbitrary`: TODO: needs guidance

- `FastCheck.Memo`: TODO: needs guidance

- `FastCheck.MixedCaseConstraints`: TODO: needs guidance

- `FastCheck.ModelRunAsyncSetup`: TODO: needs guidance

- `FastCheck.ModelRunSetup`: TODO: needs guidance

- `FastCheck.NatConstraints`: TODO: needs guidance

- `FastCheck.ObjectConstraints`: TODO: needs guidance

- `FastCheck.OneOfConstraints`: TODO: needs guidance

- `FastCheck.OneOfValue`: TODO: needs guidance

- `FastCheck.OptionConstraints`: TODO: needs guidance

- `FastCheck.Parameters`: TODO: needs guidance

- `FastCheck.PreconditionFailure`: TODO: needs guidance

- `FastCheck.PropertyHookFunction`: TODO: needs guidance

- `FastCheck.Random`: TODO: needs guidance

- `FastCheck.RandomType`: TODO: needs guidance

- `FastCheck.RecordConstraints`: TODO: needs guidance

- `FastCheck.RecordValue`: TODO: needs guidance

- `FastCheck.RunDetails`: TODO: needs guidance

- `FastCheck.RunDetailsCommon`: TODO: needs guidance

- `FastCheck.RunDetailsFailureInterrupted`: TODO: needs guidance

- `FastCheck.RunDetailsFailureProperty`: TODO: needs guidance

- `FastCheck.RunDetailsFailureTooManySkips`: TODO: needs guidance

- `FastCheck.RunDetailsSuccess`: TODO: needs guidance

- `FastCheck.SchedulerAct`: TODO: needs guidance

- `FastCheck.SchedulerConstraints`: TODO: needs guidance

- `FastCheck.SchedulerReportItem`: TODO: needs guidance

- `FastCheck.SchedulerSequenceItem`: TODO: needs guidance

- `FastCheck.ShuffledSubarrayConstraints`: TODO: needs guidance

- `FastCheck.Size`: TODO: needs guidance

- `FastCheck.SizeForArbitrary`: TODO: needs guidance

- `FastCheck.SparseArrayConstraints`: TODO: needs guidance

- `FastCheck.StringConstraints`: TODO: needs guidance

- `FastCheck.StringMatchingConstraints`: TODO: needs guidance

- `FastCheck.StringSharedConstraints`: TODO: needs guidance

- `FastCheck.SubarrayConstraints`: TODO: needs guidance

- `FastCheck.UnicodeJsonSharedConstraints` -> `FastCheck.JsonSharedConstraints`: Depend on fast-check and import it directly. Unicode JSON generation was consolidated into JsonSharedConstraints.stringUnit.

- `FastCheck.UniqueArrayConstraints`: TODO: needs guidance

- `FastCheck.UniqueArrayConstraintsCustomCompare`: TODO: needs guidance

- `FastCheck.UniqueArrayConstraintsCustomCompareSelect`: TODO: needs guidance

- `FastCheck.UniqueArrayConstraintsRecommended`: TODO: needs guidance

- `FastCheck.UniqueArraySharedConstraints`: TODO: needs guidance

- `FastCheck.UuidConstraints`: TODO: needs guidance

- `FastCheck.Value`: TODO: needs guidance

- `FastCheck.VerbosityLevel`: TODO: needs guidance

- `FastCheck.WebAuthorityConstraints`: TODO: needs guidance

- `FastCheck.WebFragmentsConstraints`: TODO: needs guidance

- `FastCheck.WebPathConstraints`: TODO: needs guidance

- `FastCheck.WebQueryParametersConstraints`: TODO: needs guidance

- `FastCheck.WebSegmentConstraints`: TODO: needs guidance

- `FastCheck.WebUrlConstraints`: TODO: needs guidance

- `FastCheck.WeightedArbitrary`: TODO: needs guidance

- `FastCheck.WithAsyncToStringMethod`: TODO: needs guidance

- `FastCheck.WithCloneMethod`: TODO: needs guidance

- `FastCheck.WithToStringMethod`: TODO: needs guidance

- `FastCheck.__commitHash`: TODO: needs guidance

- `FastCheck.__type`: TODO: needs guidance

- `FastCheck.__version`: TODO: needs guidance

- `FastCheck.anything`: TODO: needs guidance

- `FastCheck.array`: TODO: needs guidance

#### `FastCheck.ascii`

**Replacement:** `FastCheck.string`

Depend on fast-check and import it directly. fast-check v4 replaced character arbitraries with string units.

**Example**

```ts
FastCheck.string({ unit: "binary-ascii", minLength: 1, maxLength: 1 })
```

#### `FastCheck.asciiString`

**Replacement:** `FastCheck.string`

Depend on fast-check and import it directly. Use the binary-ascii string unit.

**Example**

```ts
FastCheck.string({ ...constraints, unit: "binary-ascii" })
```

- `FastCheck.assert`: TODO: needs guidance

- `FastCheck.asyncDefaultReportMessage`: TODO: needs guidance

- `FastCheck.asyncModelRun`: TODO: needs guidance

- `FastCheck.asyncProperty`: TODO: needs guidance

- `FastCheck.asyncStringify`: TODO: needs guidance

- `FastCheck.asyncToStringMethod`: TODO: needs guidance

#### `FastCheck.base64`

**Replacement:** `FastCheck.constantFrom`

Depend on fast-check and import it directly. Generate one base64 alphabet character; base64String remains for complete encoded strings.

**Example**

```ts
FastCheck.constantFrom(..."abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789+/")
```

- `FastCheck.base64String`: TODO: needs guidance

- `FastCheck.bigInt`: TODO: needs guidance

- `FastCheck.bigInt64Array`: TODO: needs guidance

- `FastCheck.bigIntN` -> `FastCheck.bigInt`: Depend on fast-check and import it directly. Express the signed bit range with min and max constraints.

#### `FastCheck.bigUint`

**Replacement:** `FastCheck.bigInt`

Depend on fast-check and import it directly. Use a minimum of 0n and the previous maximum.

**Example**

```ts
FastCheck.bigInt({ min: 0n, max })
```

- `FastCheck.bigUint64Array`: TODO: needs guidance

#### `FastCheck.bigUintN`

**Replacement:** `FastCheck.bigInt`

Depend on fast-check and import it directly. Express the unsigned bit range with min and max constraints.

**Example**

```ts
FastCheck.bigInt({ min: 0n, max: (1n << BigInt(n)) - 1n })
```

- `FastCheck.boolean`: TODO: needs guidance

#### `FastCheck.char`

**Replacement:** `FastCheck.string`

Depend on fast-check and import it directly. Use a one-unit printable ASCII string.

**Example**

```ts
FastCheck.string({ unit: "grapheme-ascii", minLength: 1, maxLength: 1 })
```

#### `FastCheck.char16bits`

**Replacement:** `FastCheck.nat`

Depend on fast-check and import it directly. Map a 16-bit natural number through String.fromCharCode.

**Example**

```ts
FastCheck.nat({ max: 0xffff }).map(String.fromCharCode)
```

- `FastCheck.check` -> `FastCheck.check`: Depend on fast-check and import it directly. The runner remains, but RunDetails.error was replaced by errorInstance in fast-check v4.

- `FastCheck.clone`: TODO: needs guidance

- `FastCheck.cloneIfNeeded`: TODO: needs guidance

- `FastCheck.cloneMethod`: TODO: needs guidance

- `FastCheck.commands`: TODO: needs guidance

- `FastCheck.compareBooleanFunc`: TODO: needs guidance

- `FastCheck.compareFunc`: TODO: needs guidance

- `FastCheck.configureGlobal`: TODO: needs guidance

- `FastCheck.constant` -> `FastCheck.constant`: Depend on fast-check and import it directly. The API remains; v4 infers literal types by default.

- `FastCheck.constantFrom`: TODO: needs guidance

- `FastCheck.context` -> `FastCheck.context`: Depend on fast-check and import it directly. The API is otherwise unchanged.

- `FastCheck.createDepthIdentifier`: TODO: needs guidance

- `FastCheck.date`: TODO: needs guidance

- `FastCheck.defaultReportMessage`: TODO: needs guidance

- `FastCheck.dictionary`: TODO: needs guidance

- `FastCheck.domain`: TODO: needs guidance

- `FastCheck.double`: TODO: needs guidance

- `FastCheck.emailAddress`: TODO: needs guidance

- `FastCheck.falsy`: TODO: needs guidance

- `FastCheck.float`: TODO: needs guidance

- `FastCheck.float32Array`: TODO: needs guidance

- `FastCheck.float64Array`: TODO: needs guidance

#### `FastCheck.fullUnicode`

**Replacement:** `FastCheck.string`

Depend on fast-check and import it directly. Use a one-unit binary Unicode string.

**Example**

```ts
FastCheck.string({ unit: "binary", minLength: 1, maxLength: 1 })
```

#### `FastCheck.fullUnicodeString`

**Replacement:** `FastCheck.string`

Depend on fast-check and import it directly. Use the binary string unit.

**Example**

```ts
FastCheck.string({ ...constraints, unit: "binary" })
```

- `FastCheck.func`: TODO: needs guidance

- `FastCheck.gen`: TODO: needs guidance

- `FastCheck.getDepthContextFor`: TODO: needs guidance

- `FastCheck.hasAsyncToStringMethod`: TODO: needs guidance

- `FastCheck.hasCloneMethod`: TODO: needs guidance

- `FastCheck.hasToStringMethod`: TODO: needs guidance

- `FastCheck.hash`: TODO: needs guidance

- `FastCheck.hexa` -> `FastCheck.integer`: Depend on fast-check and import it directly. Map an integer from 0 through 15 to a hexadecimal character.

- `FastCheck.hexaString` -> `FastCheck.string`: Depend on fast-check and import it directly. Pass a hexadecimal-character arbitrary as the string unit.

- `FastCheck.infiniteStream`: TODO: needs guidance

- `FastCheck.int16Array`: TODO: needs guidance

- `FastCheck.int32Array`: TODO: needs guidance

- `FastCheck.int8Array`: TODO: needs guidance

- `FastCheck.integer`: TODO: needs guidance

- `FastCheck.ipV4`: TODO: needs guidance

- `FastCheck.ipV4Extended`: TODO: needs guidance

- `FastCheck.ipV6`: TODO: needs guidance

- `FastCheck.json`: TODO: needs guidance

- `FastCheck.jsonValue`: TODO: needs guidance

- `FastCheck.letrec`: TODO: needs guidance

- `FastCheck.limitShrink`: TODO: needs guidance

- `FastCheck.lorem`: TODO: needs guidance

- `FastCheck.mapToConstant`: TODO: needs guidance

- `FastCheck.maxSafeInteger`: TODO: needs guidance

- `FastCheck.maxSafeNat`: TODO: needs guidance

- `FastCheck.memo`: TODO: needs guidance

- `FastCheck.mixedCase`: TODO: needs guidance

- `FastCheck.modelRun`: TODO: needs guidance

- `FastCheck.nat`: TODO: needs guidance

- `FastCheck.noBias`: TODO: needs guidance

- `FastCheck.noShrink`: TODO: needs guidance

- `FastCheck.object`: TODO: needs guidance

- `FastCheck.oneof`: TODO: needs guidance

- `FastCheck.option`: TODO: needs guidance

- `FastCheck.pre`: TODO: needs guidance

- `FastCheck.property`: TODO: needs guidance

- `FastCheck.readConfigureGlobal`: TODO: needs guidance

- `FastCheck.record`: TODO: needs guidance

- `FastCheck.resetConfigureGlobal` -> `Undici.install`: TODO: needs guidance

- `FastCheck.sample`: TODO: needs guidance

- `FastCheck.scheduledModelRun`: TODO: needs guidance

- `FastCheck.scheduler`: TODO: needs guidance

- `FastCheck.schedulerFor`: TODO: needs guidance

- `FastCheck.shuffledSubarray`: TODO: needs guidance

- `FastCheck.sparseArray`: TODO: needs guidance

- `FastCheck.statistics`: TODO: needs guidance

- `FastCheck.stream` -> `FastCheck.stream`: Depend on fast-check and import it directly. The API remains; update custom generator and Random implementations for fast-check v4 typings.

- `FastCheck.string`: TODO: needs guidance

- `FastCheck.string16bits` -> `FastCheck.string`: Depend on fast-check and import it directly. Pass a char16bits-compatible arbitrary as the string unit.

- `FastCheck.stringMatching`: TODO: needs guidance

#### `FastCheck.stringOf`

**Replacement:** `FastCheck.string`

Depend on fast-check and import it directly. Pass the former character arbitrary as the unit constraint.

**Example**

```ts
FastCheck.string({ ...constraints, unit: arbitrary })
```

- `FastCheck.subarray`: TODO: needs guidance

- `FastCheck.toStringMethod`: TODO: needs guidance

- `FastCheck.tuple`: TODO: needs guidance

- `FastCheck.uint16Array`: TODO: needs guidance

- `FastCheck.uint32Array`: TODO: needs guidance

- `FastCheck.uint8Array`: TODO: needs guidance

- `FastCheck.uint8ClampedArray`: TODO: needs guidance

- `FastCheck.ulid`: TODO: needs guidance

- `FastCheck.unicode` -> `FastCheck.integer`: Depend on fast-check and import it directly. Map BMP code points while excluding surrogate code points; prefer the binary string unit for full Unicode.

#### `FastCheck.unicodeJson`

**Replacement:** `FastCheck.json`

Depend on fast-check and import it directly. Select binary or grapheme strings with stringUnit.

**Example**

```ts
FastCheck.json({ stringUnit: "binary" })
```

#### `FastCheck.unicodeJsonValue`

**Replacement:** `FastCheck.jsonValue`

Depend on fast-check and import it directly. Select binary or grapheme strings with stringUnit.

**Example**

```ts
FastCheck.jsonValue({ stringUnit: "binary" })
```

- `FastCheck.unicodeString` -> `FastCheck.string`: Depend on fast-check and import it directly. Pass a BMP-code-point arbitrary as the unit constraint; prefer unit: binary for full Unicode.

- `FastCheck.uniqueArray`: TODO: needs guidance

- `FastCheck.uuid`: TODO: needs guidance

#### `FastCheck.uuidV`

**Replacement:** `FastCheck.uuid`

Depend on fast-check and import it directly. Specify the UUID version through constraints.

**Example**

```ts
FastCheck.uuid({ version: 4 })
```

- `FastCheck.webAuthority`: TODO: needs guidance

- `FastCheck.webFragments`: TODO: needs guidance

- `FastCheck.webPath`: TODO: needs guidance

- `FastCheck.webQueryParameters`: TODO: needs guidance

- `FastCheck.webSegment`: TODO: needs guidance

- `FastCheck.webUrl`: TODO: needs guidance
