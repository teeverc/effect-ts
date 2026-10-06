# v3 → v4 API map: effect/Inspectable

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Inspectable`

- `Inspectable.format` -> `Formatter.formatJson`: Use Formatter.formatJson(input, { space: 2 }) to preserve the v3 helper's pretty-printed JSON output.

- `Inspectable.redact` -> `Redactable.redact`: The redaction protocol moved to Redactable and now receives the current fiber Context.

- `Inspectable.stringifyCircular` -> `Formatter.formatJson`: Use Formatter.formatJson(input, { space: whitespace }); it handles redaction and ancestor cycles.

- `Inspectable.toJSON` -> `Inspectable.toJson`: Renamed to lower-camel toJson with the same recursive conversion role.

- `Inspectable.withRedactableContext` -> `none`: Manual FiberRefs scoping was removed; Redactable.redact uses the current fiber Context automatically.
