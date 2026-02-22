# Resource Management (Scope) - v4

Use this guide when acquiring and releasing resources.

- `Scope` provides resource management for effects.
- Closing a scope releases all resources attached to it.
- Add finalizers to a scope to define cleanup logic.
- Prefer scoped acquisition for files, sockets, and other resources that must be released deterministically.

## Mental model

- `Scope` tracks acquired resources and runs finalizers in LIFO order.
- `Effect.acquireRelease` defines acquire + release; `Effect.scoped` runs it safely.
- Release runs on success, failure, or interruption; use the `exit` value to customize cleanup.
- Use `forkScoped` for background work that uses scoped resources.

## v4 Change: `Scope.extend` → `Scope.provide`

`Scope.extend` has been renamed to `Scope.provide`. It provides a `Scope` to an effect without closing the scope when the effect completes.

```ts
import * as Effect from "effect/Effect"
import * as Scope from "effect/Scope"

// v4
const program = Effect.gen(function*() {
  const scope = yield* Scope.make()
  yield* Scope.provide(scope)(myEffect)
  // scope stays open after myEffect completes
})

// Both forms supported:
Scope.provide(myEffect, scope)       // data-first
myEffect.pipe(Scope.provide(scope))  // data-last (curried)
```

## Walkthrough: manage a resource

```ts
import * as Effect from "effect/Effect"

const openFile = (path: string) => Effect.sync(() => ({ path }))
const closeFile = (file: { path: string }) => Effect.sync(() => undefined)

const program = Effect.scoped(
  Effect.gen(function*() {
    const file = yield* Effect.acquireRelease(
      openFile("/tmp/app.log"),
      (file, _exit) => closeFile(file)
    )

    return file.path
  })
)
```

## Wiring guide

- Wrap resources in Layers using `Layer.scoped` so dependents never see raw handles.
- Use `Effect.scoped` around any block that allocates resources.
- When using concurrency, ensure fibers that touch scoped resources are created with `forkScoped`.
- Use `Scope.provide` (v4) when manually managing scope lifetimes.

## Pitfalls

- Using removed `Scope.extend` (renamed to `Scope.provide` in v4).
- Using a scoped resource after the scope closes.
- Forgetting to call `Effect.scoped` around `acquireRelease`.
- Finalizers that can fail silently (log or handle errors explicitly).

## Docs

- `https://effect.website/docs/resource-management/introduction/`
- `https://effect.website/docs/resource-management/scope/`
