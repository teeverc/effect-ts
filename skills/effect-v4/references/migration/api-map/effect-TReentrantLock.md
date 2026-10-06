# v3 → v4 API map: effect/TReentrantLock

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/TReentrantLock`

- `TReentrantLock.TReentrantLock` -> `TxReentrantLock.TxReentrantLock`: Rename the type and import from "effect/TxReentrantLock". V4 Tx operations return ordinary Effects; compose multiple operations under one outer Effect.tx to keep them atomic.

- `TReentrantLock.TReentrantLock.Proto` -> `none`: The public prototype interface was removed.

- `TReentrantLock.TReentrantLockTypeId` -> `TxReentrantLock.isTxReentrantLock`: The type id is internal in v4; use the public runtime guard.

- `TReentrantLock.fiberReadLocks` -> `none`: Per-fiber read-lock counts were removed; TxReentrantLock.readLocks reports only the total count.

- `TReentrantLock.fiberWriteLocks` -> `none`: Per-fiber write-lock counts were removed; TxReentrantLock.writeLocks reports only the total count.

- `TReentrantLock.lock` -> `TxReentrantLock.writeLock`: The generic lock helper was renamed to make write-lock acquisition explicit.

- `TReentrantLock.make` -> `TxReentrantLock.make()`: The constructor keeps its name but is now a function call rather than a constant STM value.
