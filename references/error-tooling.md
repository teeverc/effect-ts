# Error Tooling (Cause and Sandboxing)

Use this guide when you need to inspect or manipulate failures.

- Use `Effect.sandbox` to expose defects as `Cause` in the error channel.
- Use `Effect.unsandbox` to restore defects to the defect channel.
- Use `Effect.cause` to access `Cause<E>` and `Effect.either` for `Either<E, A>`.
- Use `Effect.catchAllCause` to handle typed errors and defects together.
- Prefer `Cause`-aware handlers for diagnostics and reporting.
