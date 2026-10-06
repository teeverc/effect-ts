# v3 → v4 API map: effect/ConfigProvider

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/ConfigProvider`

- `ConfigProvider.ConfigProvider` -> `ConfigProvider.ConfigProvider`: The model remains but now exposes `load(path)`, returning `Effect<Node | undefined, SourceError>`, and `mapInput(f)` for provider-owned path transformation. `undefined` means the path is missing; a `Node` means it exists.

- `ConfigProvider.ConfigProvider.Flat` -> `ConfigProvider.ConfigProvider`: Flat providers were removed; implement the unified path-based provider with ConfigProvider.make.

- `ConfigProvider.ConfigProvider.FromEnvConfig` -> `Parameters<typeof ConfigProvider.fromEnv>[0]`: Options are inline in v4 and contain env plus preserveEmptyStrings; custom path delimiters moved to provider path transforms, while separated sequences and records use Config.Array and Config.Record.

- `ConfigProvider.ConfigProvider.FromMapConfig` -> `none`: fromMap and its delimiter options were removed; expand delimited keys into a nested value and use ConfigProvider.fromUnknown.

- `ConfigProvider.ConfigProvider.KeyComponent` -> `ConfigProvider.Path[number]`: Tagged key components became plain string or number path segments.

- `ConfigProvider.ConfigProvider.KeyIndex` -> `number`: Tagged KeyIndex values became numeric ConfigProvider.Path segments.

- `ConfigProvider.ConfigProvider.KeyName` -> `string`: Tagged KeyName values became string ConfigProvider.Path segments.

- `ConfigProvider.ConfigProvider.Proto` -> `ConfigProvider.ConfigProvider`: The public marker prototype was removed; use the provider interface itself.

- `ConfigProvider.ConfigProviderTypeId` -> `ConfigProvider.ConfigProvider`: The runtime marker is private in v4; providers are created by public constructors and consumed structurally.

- `ConfigProvider.FlatConfigProviderTypeId` -> `none`: The flat-provider abstraction and marker were removed.

- `ConfigProvider.fromEnv` -> `ConfigProvider.fromEnv`: The constructor remains; pass env and preserveEmptyStrings options. Paths use underscore semantics, while sequence separators belong on Config schemas.

- `ConfigProvider.fromFlat` -> `ConfigProvider.make`: Flat providers were unified with ConfigProvider; return a `Value`, `Record`, or `Array` node for a found path, or `undefined` when missing.

- `ConfigProvider.fromJson` -> `ConfigProvider.fromUnknown`: Renamed to reflect support for any in-memory JavaScript value.

- `ConfigProvider.fromMap` -> `ConfigProvider.fromUnknown`: Expand the map's delimited keys into a nested object first; v4 removed fromMap and its pathDelim/seqDelim options.

- `ConfigProvider.kebabCase` -> `ConfigProvider.mapInput((path) => path.map((part) => typeof part === "string" ? String.kebabCase(part) : part))`: Named recasing helpers were removed except constantCase; transform string path segments explicitly.

- `ConfigProvider.lowerCase` -> `ConfigProvider.mapInput((path) => path.map((part) => typeof part === "string" ? part.toLowerCase() : part))`: Transform string path segments explicitly with mapInput.

- `ConfigProvider.make` -> `ConfigProvider.make`: The constructor now takes a path lookup returning `Effect<Node | undefined, SourceError>`, rather than a full Config loader and flattened provider. Return `undefined` for a missing path and a `Node` for a found path.

- `ConfigProvider.makeFlat` -> `ConfigProvider.make`: The flat-provider constructor was removed; return a `Value`, `Record`, or `Array` node for a found path, or `undefined` when missing.

- `ConfigProvider.mapInputPath` -> `ConfigProvider.mapInput`: Renamed and generalized: the callback receives and returns the complete Path, including numeric array indexes.

- `ConfigProvider.snakeCase` -> `ConfigProvider.mapInput((path) => path.map((part) => typeof part === "string" ? String.snakeCase(part) : part))`: Named recasing helpers were removed except constantCase; transform string path segments explicitly.

- `ConfigProvider.unnested` -> `ConfigProvider.mapInput((path) => path[0] === name ? path.slice(1) : path)`: The named helper was removed; strip the matching leading segment explicitly. Add custom handling if the v3 mismatch error was significant.

- `ConfigProvider.upperCase` -> `ConfigProvider.mapInput((path) => path.map((part) => typeof part === "string" ? part.toUpperCase() : part))`: Transform string path segments explicitly with mapInput.

- `ConfigProvider.within` -> `ConfigProvider.orElse + ConfigProvider.mapInput`: The scoped transform helper was removed; build a provider that transforms paths below the prefix and falls back to the original provider elsewhere.
