# Concurrency Advanced (Interruption, Supervision, FiberRef)

Use this guide when coordinating fibers beyond simple forking.

- Effects run on fibers; use `Effect.fork` to start child fibers and `Fiber.join`/`Fiber.interrupt` to coordinate.
- Interruption is cooperative; use `Effect.onInterrupt` for cleanup.
- Use `Effect.race`, `Effect.raceAll`, `Effect.raceFirst`, or `Effect.raceWith` for races.
- Control parallelism with `Effect.all` concurrency options (`inherit`, bounded numbers, or `unbounded`).
- Use `FiberRef` for fiber-local values; use `FiberRef.get`/`FiberRef.set`/`FiberRef.update` to read and update.
- Use supervision to keep child fibers tied to a parent scope so they are interrupted when the parent exits.
- Prefer structured concurrency: keep child fibers tied to a scope unless you intentionally detach.
