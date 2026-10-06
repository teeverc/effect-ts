# v3 → v4 API map: effect/Cron

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Cron`

- `Cron.Cron` -> `Cron.Cron`: The model remains; update for the v4 representation and private type id.

- `Cron.ParseError` -> `Cron.CronParseError`: The parse error was renamed and Cron.parse now returns Result.Result\<Cron, CronParseError\>.

- `Cron.ParseErrorTypeId` -> `none`: The cron parse-error type id is private in v4. Use Cron.isCronParseError to narrow unknown failures.

- `Cron.TypeId` -> `none`: The Cron type id is private in v4. Use Cron.isCron to identify cron values.

- `Cron.isParseError` -> `Cron.isCronParseError`: The parse-error guard was renamed with the error type.

- `Cron.sequenceReverse` -> `Cron.prev`: The reverse iterator was removed. Build an iterator that repeatedly calls Cron.prev, feeding each returned Date into the next call.

- `Cron.unsafeParse` -> `Cron.parseUnsafe`: The throwing parser was renamed; it also accepts an optional time zone.
