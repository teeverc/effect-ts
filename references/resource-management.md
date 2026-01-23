# Resource Management (Scope)

Use this guide when acquiring and releasing resources.

- `Scope` provides resource management for effects.
- Closing a scope releases all resources attached to it.
- Add finalizers to a scope to define cleanup logic.
- Prefer scoped acquisition for files, sockets, and other resources that must be released deterministically.

## Example
```ts
import * as Effect from "effect/Effect"

const program = Effect.acquireRelease(
  Effect.sync(() => ({
    read: () => "data",
    close: () => {}
  })),
  (resource) => Effect.sync(() => resource.close())
).pipe(
  Effect.flatMap((resource) => Effect.sync(() => resource.read()))
)

Effect.runPromise(Effect.scoped(program))
```
