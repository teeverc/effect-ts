# v3 → v4 API map: effect/DateTime

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/DateTime`

- `DateTime.DateTime` -> `DateTime.DateTime`: The Utc | Zoned model remains; epochMillis fields are now epochMilliseconds and unit/part names use millisecond terminology.

- `DateTime.DateTime.Input` -> `DateTime.DateTime.Input`: The input type remains and additionally accepts Instant and InstantWithZone objects.

- `DateTime.DateTime.Parts` -> `DateTime.DateTime.Parts`: Rename millis, seconds, minutes, and hours fields to millisecond, second, minute, and hour.

- `DateTime.DateTime.PartsForMath` -> `DateTime.DateTime.PartsForMath`: Rename the millis field to milliseconds; the other plural arithmetic fields remain.

- `DateTime.DateTime.PartsWithWeekday` -> `DateTime.DateTime.PartsWithWeekday`: Rename millis, seconds, minutes, and hours fields to millisecond, second, minute, and hour.

- `DateTime.DateTime.Proto` -> `DateTime.DateTime.Proto`: The protocol remains, but its marker uses the private v4 TypeId value.

- `DateTime.DateTime.UnitPlural` -> `DateTime.DateTime.UnitPlural`: Use milliseconds instead of millis; the other plural unit strings remain.

- `DateTime.DateTime.UnitSingular` -> `DateTime.DateTime.UnitSingular`: Use millisecond instead of milli; the other singular unit strings remain.

- `DateTime.TimeZone` -> `DateTime.TimeZone`: The Offset | Named model remains; its public type-id marker type was removed.

- `DateTime.TimeZone.Proto` -> `DateTime.TimeZone.Proto`: The protocol remains, but its marker uses the private v4 TimeZoneTypeId value.

- `DateTime.TimeZoneTypeId` -> `none`: The time-zone type id is private in v4. Use DateTime.isTimeZone, isTimeZoneOffset, or isTimeZoneNamed.

- `DateTime.TypeId` -> `none`: The DateTime type id is private in v4. Use DateTime.isDateTime, isUtc, or isZoned.

- `DateTime.Utc` -> `DateTime.Utc`: The model remains; rename epochMillis to epochMilliseconds.

- `DateTime.Zoned` -> `DateTime.Zoned`: The model remains; rename epochMillis and adjustedEpochMillis to epochMilliseconds and adjustedEpochMilliseconds.

- `DateTime.distanceDuration` -> `Duration.millis(Math.abs(DateTime.distance(self, other)))`: DateTime.distance returns signed milliseconds in v4; take the absolute value and construct a Duration to preserve v3 behavior.

- `DateTime.distanceDurationEither` -> `DateTime.distance + Result`: Compute the signed millisecond distance, wrap its absolute Duration as Result.succeed when positive and Result.fail when non-positive; v4 uses Result instead of Either.

- `DateTime.greaterThan` -> `DateTime.isGreaterThan`: The comparison was renamed with the is prefix.

- `DateTime.greaterThanOrEqualTo` -> `DateTime.isGreaterThanOrEqualTo`: The comparison was renamed with the is prefix.

- `DateTime.lessThan` -> `DateTime.isLessThan`: The comparison was renamed with the is prefix.

- `DateTime.lessThanOrEqualTo` -> `DateTime.isLessThanOrEqualTo`: The comparison was renamed with the is prefix.

- `DateTime.unsafeFromDate` -> `DateTime.fromDateUnsafe`: The unsafe suffix moved to the end of the constructor name.

- `DateTime.unsafeIsFuture` -> `DateTime.isFutureUnsafe`: The unsafe suffix moved to the end of the predicate name.

- `DateTime.unsafeIsPast` -> `DateTime.isPastUnsafe`: The unsafe suffix moved to the end of the predicate name.

- `DateTime.unsafeMake` -> `DateTime.makeUnsafe`: The unsafe suffix moved to the end of the constructor name.

- `DateTime.unsafeMakeZoned` -> `DateTime.makeZonedUnsafe`: The unsafe suffix moved to the end of the constructor name.

- `DateTime.unsafeNow` -> `DateTime.nowUnsafe`: The unsafe suffix moved to the end of the accessor name.

- `DateTime.unsafeSetZoneNamed` -> `DateTime.setZoneNamedUnsafe`: The unsafe suffix moved to the end of the zone setter name.

- `DateTime.zoneUnsafeMakeNamed` -> `DateTime.zoneMakeNamedUnsafe`: The unsafe suffix moved to the end of the named-zone constructor.
