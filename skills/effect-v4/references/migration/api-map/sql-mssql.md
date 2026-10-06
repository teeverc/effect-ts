# v3 → v4 API map: @effect/sql-mssql/MssqlClient, @effect/sql-mssql/Parameter, @effect/sql-mssql/Procedure

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/sql-mssql/MssqlClient`

- `MssqlClient.MssqlClient` -> `@effect/sql-mssql/MssqlClient#MssqlClient`: Retained; the service value is now a Context.Service rather than a GenericTag.

- `MssqlClient.MssqlClientConfig` -> `@effect/sql-mssql/MssqlClient#MssqlClientConfig`: Retained; durations use Duration.Input, parameterTypes is keyed by Statement.PrimitiveKind, and v4 adds retry and timeout options.

## `@effect/sql-mssql/Parameter`

- `Parameter.Parameter` -> `@effect/sql-mssql/Parameter#Parameter`: Retained; the phantom brand key was renamed from ParameterId to TypeId.

- `Parameter.ParameterId` -> `@effect/sql-mssql/Parameter#TypeId`: Renamed; use TypeId for direct brand-key and type references.

## `@effect/sql-mssql/Procedure`

- `Procedure.Procedure` -> `@effect/sql-mssql/Procedure#Procedure`: Retained with the same generics and fields.

- `Procedure.Procedure.ParametersRecord` -> `@effect/sql-mssql/Procedure#Procedure.ParametersRecord`: Retained unchanged; from the deep module it is also available as Procedure.ParametersRecord.
