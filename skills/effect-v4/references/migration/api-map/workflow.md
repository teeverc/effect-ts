# v3 → v4 API map: @effect/workflow (8 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/workflow/Activity`

- `Activity.Any` -> `effect/workflow/Activity#Any`: Moved into core Effect. V4 Any is minimal; use AnyWithProps when schemas or execution properties are required.

- `Activity.CurrentAttempt` -> `effect/workflow/Activity#CurrentAttempt`: Moved into core Effect and changed from a Context.Tag subclass to a Context.Reference value with the same default of 1.

- `Activity.TypeId` -> `none`: The activity marker is private in v4. Use Activity, Activity.Any, or Activity.AnyWithProps constraints.

- `Activity.make` -> `effect/workflow/Activity#make`: Moved into core Effect with the same constructor shape, v4 Schema.Constraint service directions, and optional annotations.

- `Activity.raceAll` -> `effect/workflow/Activity#raceAll`: Moved into core Effect with the same named durable race behavior.

## `@effect/workflow/DurableClock`

- `DurableClock.TypeId` -> `none`: The durable-clock marker is private in v4. Use DurableClock values structurally.

- `DurableClock.make` -> `effect/workflow/DurableClock#make`: Moved into core Effect; Duration.DurationInput is now Duration.Input.

## `@effect/workflow/DurableDeferred`

- `DurableDeferred.Any` -> `effect/workflow/DurableDeferred#Any`: Moved into core Effect. V4 Any is minimal; use AnyWithProps when success, error, or exit schemas are required.

- `DurableDeferred.TypeId` -> `none`: The durable-deferred marker is private in v4. Use DurableDeferred, Any, or AnyWithProps constraints.

- `DurableDeferred.await` -> `effect/workflow/DurableDeferred#await`: Moved into core Effect with the same persisted-result and workflow-suspension behavior.

- `DurableDeferred.done` -> `effect/workflow/DurableDeferred#done`: Moved into core Effect; schema requirements now use explicit directional encoding services.

- `DurableDeferred.fail` -> `effect/workflow/DurableDeferred#fail`: Moved into core Effect and now requires the error schema encoding services.

- `DurableDeferred.failCause` -> `effect/workflow/DurableDeferred#failCause`: Moved into core Effect and now requires the error schema encoding services.

- `DurableDeferred.into` -> `effect/workflow/DurableDeferred#into`: Moved into core Effect with the same exit recording and suspension propagation behavior. Provide both decoding and encoding services for the success and error schemas; recording the exit requires encoding services.

- `DurableDeferred.make` -> `effect/workflow/DurableDeferred#make`: Moved into core Effect with the same name and optional schemas, expressed through v4 Schema.Constraint.

- `DurableDeferred.raceAll` -> `effect/workflow/DurableDeferred#raceAll`: Moved into core Effect with the same persisted-winner behavior.

- `DurableDeferred.succeed` -> `effect/workflow/DurableDeferred#succeed`: Moved into core Effect and now requires the success schema encoding services.

## `@effect/workflow/DurableQueue`

- `DurableQueue.TypeId` -> `effect/workflow/DurableQueue#TypeId`: Moved into core Effect; the marker literal changed to \~effect/workflow/DurableQueue. Use typeof DurableQueue.TypeId in type position.

- `DurableQueue.make` -> `effect/workflow/DurableQueue#make`: Moved into core Effect; queue persistence now comes from effect/persistence.

## `@effect/workflow/Workflow`

- `Workflow.Any` -> `effect/workflow/Workflow#Any`: Moved into core Effect. Workflow identity changed from name to \_tag and definitions are now class-compatible constructors.

- `Workflow.AnyTaggedRequestSchema` -> `none`: The TaggedRequest adapter constraint was removed. Define the workflow explicitly with Workflow.make and the request payload, success, error, and PrimaryKey schemas.

- `Workflow.CaptureDefects` -> `effect/workflow/Workflow#CaptureDefects`: Moved into core Effect and changed from a Context.Tag subclass to a Context.Reference value with the same true default.

- `Workflow.Execution` -> `effect/workflow/Workflow#Execution`: Moved into core Effect; its workflow discriminator changed from name to \_tag.

- `Workflow.Requirements` -> `Workflow.RequirementsClient / Workflow.RequirementsHandler`: The schema Context union split by direction: client payload encoding and result decoding versus handler payload decoding and result encoding.

- `Workflow.Result` -> `effect/workflow/Workflow#Result`: Moved into core Effect and remains the Complete or Suspended result type and schema constructor.

- `Workflow.ResultEncoded` -> `effect/workflow/Workflow#ResultEncoded`: Moved into core Effect and remains both the encoded result type and generic encoded-result codec.

- `Workflow.ResultTypeId` -> `none`: The result marker is private in v4. Use Workflow.isResult for narrowing.

- `Workflow.SuspendOnFailure` -> `effect/workflow/Workflow#SuspendOnFailure`: Moved into core Effect and changed from a Context.Tag subclass to a Context.Reference value with the same false default.

- `Workflow.TypeId` -> `none`: The workflow marker is private in v4. Use Workflow.Any or Workflow.Workflow constraints.

- `Workflow.Workflow` -> `effect/workflow/Workflow#Workflow`: Name and name became Tag and \_tag, schemas use directional services, definitions are constructable, and poll returns Option.

- `Workflow.Workflow.Error` -> `W["errorSchema"]["Type"]`: The namespace alias was removed. Extract the decoded error type from the public errorSchema property.

- `Workflow.Workflow.Payload` -> `Schema.Schema.Type<Workflow.PayloadSchema<W>>`: The namespace alias was removed. Extract the decoded payload from the exported PayloadSchema helper.

- `Workflow.Workflow.Success` -> `W["successSchema"]["Type"]`: The namespace alias was removed. Extract the decoded success type from the public successSchema property.

- `Workflow.fromTaggedRequest` -> `none`: Removed. Expand to Workflow.make(schema.\_tag, { payload: schema, success: schema.success, error: schema.failure, idempotencyKey: PrimaryKey.value }).

- `Workflow.make` -> `effect/workflow/Workflow#make`: The signature changed from make({ name, ... }) to make(tag, { ... }); definitions expose \_tag and are class-compatible constructors.

## `@effect/workflow/WorkflowEngine`

- `WorkflowEngine.makeUnsafe` -> `effect/workflow/WorkflowEngine#makeUnsafe`: Moved into core Effect. Context service projections now use Service instead of Type, and absent encoded results use Option.

## `@effect/workflow/WorkflowProxy`

- `WorkflowProxy.ConvertHttpApi` -> `effect/workflow/WorkflowProxy#ConvertHttpApi`: Moved into core Effect and updated to v4 HttpApiEndpoint types and the consolidated HttpApi architecture.

- `WorkflowProxy.ConvertRpcs` -> `effect/workflow/WorkflowProxy#ConvertRpcs`: Moved into core Effect; generated execute, discard, and resume RPCs are now keyed from workflow \_tag.

## `@effect/workflow/WorkflowProxyServer`

- `WorkflowProxyServer.layerHttpApi` -> `effect/workflow/WorkflowProxyServer#layerHttpApi`: Moved into core Effect. Use v4 HttpApi group identifiers and Workflow.RequirementsHandler schema services.

- `WorkflowProxyServer.layerRpcHandlers` -> `effect/workflow/WorkflowProxyServer#layerRpcHandlers`: Moved into core Effect; generated handlers require Workflow.RequirementsHandler rather than the undirected Requirements union.
