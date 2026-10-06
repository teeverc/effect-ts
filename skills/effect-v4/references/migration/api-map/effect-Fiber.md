# v3 → v4 API map: effect/Fiber

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Fiber`

- `Fiber.Fiber` -> `Fiber.Fiber`: The v4 Fiber is the concrete runtime handle and is no longer itself an Effect; use Fiber.join or Fiber.await.

- `Fiber.Fiber.Descriptor` -> `none`: Descriptors were removed; use Effect.withFiber for the current Fiber and read its id or runtime fields directly.

- `Fiber.Fiber.Dump` -> `none`: Fiber dumps were removed; retain explicit Fiber handles and inspect their public runtime fields.

- `Fiber.Fiber.Runtime` -> `Fiber.Fiber`: RuntimeFiber and the Fiber.Runtime alias were collapsed into the single v4 Fiber type.

- `Fiber.Fiber.RuntimeVariance` -> `Fiber.Variance`: RuntimeFiber was collapsed into Fiber, leaving one variance encoding.

- `Fiber.Fiber.Variance` -> `Fiber.Variance`: Retained as the variance encoding on the v4 Fiber interface.

- `Fiber.FiberTypeId` -> `Fiber.isFiber`: The type-id symbol is private in v4; use the public Fiber.isFiber guard.

- `Fiber.FiberUnify` -> `none`: Fiber no longer extends Effect, so its Effect unification helper was removed.

- `Fiber.FiberUnifyIgnore` -> `none`: Fiber no longer extends Effect, so its Effect unification helper was removed.

- `Fiber.Order` -> `Order.mapInput(Order.Number, (fiber) => fiber.id)`: The built-in Fiber order was removed; derive an order from the numeric id when ordering is actually required.

- `Fiber.RuntimeFiber` -> `Fiber.Fiber`: RuntimeFiber and Fiber were collapsed into the single v4 Fiber interface.

- `Fiber.RuntimeFiberTypeId` -> `Fiber.isFiber`: The separate RuntimeFiber marker was removed; use the public Fiber guard.

- `Fiber.RuntimeFiberUnify` -> `none`: RuntimeFiber was collapsed into Fiber, which no longer participates in Effect unification.

- `Fiber.RuntimeFiberUnifyIgnore` -> `none`: RuntimeFiber was collapsed into Fiber, which no longer participates in Effect unification.

- `Fiber.all` -> `Fiber.joinAll`: Composite fibers were removed; join the iterable directly to obtain an Effect of all results.

- `Fiber.await` -> `Fiber.await`: Unchanged; it returns an Effect containing the fiber Exit.

- `Fiber.children` -> `none`: V4 fibers do not expose child-fiber enumeration; keep explicit handles in FiberSet or FiberMap when tracking is required.

- `Fiber.done` -> `Effect.runFork`: Exit is an Effect in v4, so pass the Exit to Effect.runFork when a completed Fiber handle is required.

- `Fiber.dumpAll` -> `none`: Fiber dump and global diagnostic APIs were removed; retain explicit fibers and inspect id and pollUnsafe where needed.

- `Fiber.fail` -> `Effect.runFork(Effect.fail(error))`: Synthetic Fiber constructors were removed; run the corresponding Effect when a Fiber handle is required.

- `Fiber.failCause` -> `Effect.runFork(Effect.failCause(cause))`: Synthetic Fiber constructors were removed; run the corresponding Effect when a Fiber handle is required.

- `Fiber.fromEffect` -> `Effect.runFork`: V4 uses concrete runtime fibers; run the Effect directly, or keep using the Effect when no handle is needed.

- `Fiber.getCurrentFiber` -> `Fiber.getCurrent`: Renamed and now returns Fiber | undefined synchronously instead of Option.

- `Fiber.id` -> `fiber.id`: Fiber IDs are numbers exposed by the readonly id field.

- `Fiber.inheritAll` -> `none`: FiberRef inheritance was removed with FiberRef; Context.Reference values are inherited through fiber context automatically.

- `Fiber.interruptAsFork` -> `fiber.interruptUnsafe(fiberId)`: For fire-and-forget interruption use the immediate runtime hook; use Fiber.interruptAs when cleanup must be awaited.

- `Fiber.interruptFork` -> `fiber.interruptUnsafe()`: Use the immediate runtime hook for fire-and-forget interruption; Fiber.interrupt waits for cleanup.

- `Fiber.interrupted` -> `Effect.runFork(Exit.interrupt(fiberId))`: Synthetic Fiber constructors were removed; Exit is an Effect and can be run to obtain an interrupted Fiber.

- `Fiber.isRuntimeFiber` -> `Fiber.isFiber`: All v4 Fiber values are concrete runtime fibers, so only the general guard remains.

- `Fiber.map` -> `Effect.runFork(Effect.map(Fiber.join(fiber), f))`: Fiber transformation combinators were removed; transform its joined Effect and fork only if another handle is required.

- `Fiber.mapEffect` -> `Effect.runFork(Effect.flatMap(Fiber.join(fiber), f))`: Fiber transformation combinators were removed; transform its joined Effect and fork only if another handle is required.

- `Fiber.mapFiber` -> `Effect.flatMap(Fiber.join(fiber), (a) => Fiber.join(f(a)))`: Flatten through Fiber.join; fork the resulting Effect if another Fiber handle is required.

- `Fiber.match` -> `none`: The virtual Fiber versus RuntimeFiber distinction no longer exists, so branch-specific matching is unnecessary.

- `Fiber.never` -> `Effect.runFork(Effect.never)`: Synthetic Fiber constants were removed; run Effect.never when a never-completing Fiber is required.

- `Fiber.orElse` -> `Effect.runFork(Effect.catchCause(Fiber.join(self), () => Fiber.join(that)))`: Compose joined Effects and fork the result only if another Fiber handle is required.

- `Fiber.orElseEither` -> `Effect.catchCause`: Compose Fiber.join Effects explicitly and map each successful branch to your own tagged union; Either was also removed in v4.

- `Fiber.poll` -> `fiber.pollUnsafe()`: Polling is now synchronous and returns Exit | undefined; wrap in Effect.sync and Option.fromUndefinedOr if the old shape is required.

- `Fiber.pretty` -> `none`: Runtime fiber pretty-printing was removed; format the public id and polled Exit explicitly.

- `Fiber.roots` -> `none`: The runtime no longer exposes a global root-fiber registry; track application fibers explicitly.

- `Fiber.scoped` -> `Fiber.runIn`: Register the Fiber in an explicit Scope with Fiber.runIn; acquire the current Scope when migrating the old effectful form.

- `Fiber.status` -> `fiber.pollUnsafe()`: FiberStatus was removed; undefined means not completed and an Exit means completed, with no public running/suspended distinction.

- `Fiber.succeed` -> `Effect.runFork(Effect.succeed(value))`: Synthetic Fiber constructors were removed; run the corresponding Effect when a Fiber handle is required.

- `Fiber.unsafeRoots` -> `none`: The runtime no longer exposes a global root-fiber registry; track application fibers explicitly.

- `Fiber.void` -> `Effect.runFork(Effect.void)`: Synthetic Fiber constants were removed; run Effect.void when a completed Fiber\<void\> is required.

- `Fiber.zip` -> `Effect.runFork(Effect.zip(Fiber.join(self), Fiber.join(that)))`: Compose joined Effects and fork the result only if another Fiber handle is required.

- `Fiber.zipLeft` -> `Effect.runFork(Effect.map(Effect.zip(Fiber.join(self), Fiber.join(that)), ([left]) => left))`: V4 has no Effect.zipLeft; zip joined Effects, project the left value, and fork only if another handle is required.

- `Fiber.zipRight` -> `Effect.runFork(Effect.map(Effect.zip(Fiber.join(self), Fiber.join(that)), ([, right]) => right))`: V4 has no Effect.zipRight; zip joined Effects, project the right value, and fork only if another handle is required.

- `Fiber.zipWith` -> `Effect.runFork(Effect.zipWith(Fiber.join(self), Fiber.join(that), f))`: Compose joined Effects and fork the result only if another Fiber handle is required.
