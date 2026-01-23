# Troubleshooting (Common Errors and Fixes)

Use this guide when a task fails or produces confusing runtime behavior.

## Common pitfalls
- **Effect not running**: you likely built an effect but never called a `run*` function at the program edge.
- **Missing environment / service not found**: ensure required services are provided with layers before running.
- **Unhandled errors**: make sure the error channel type matches the expected domain errors; use `catchAll`/`match`.
- **Unexpected defect**: use `Effect.sandbox` and inspect the `Cause` to diagnose defects.
- **Resource leaks**: ensure resources are acquired with `Scope`/`Layer.scoped` and closed on scope finalization.
- **Fiber leaks**: prefer structured concurrency; avoid daemon fibers unless truly detached.
- **Test flakiness**: control time with `TestClock` and provide deterministic config/providers.

## Diagnostic steps
1. Inspect the effect type (`Effect<A, E, R>`) for missing environment or error types.
2. Use `Effect.tap` to log intermediate values.
3. Use `Effect.catchAllCause` to log or format causes.
4. If using layers, print or log layer composition and ensure all tags are provided.
