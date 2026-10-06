# v3 → v4 API map: @effect/platform-browser (6 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/platform-browser/BrowserHttpClient`

- `BrowserHttpClient.currentXHRResponseType` -> `BrowserHttpClient.CurrentXHRResponseType`: The FiberRef became a defaulted Context.Reference; use withXHRArrayBuffer or provide the reference as a service.

## `@effect/platform-browser/BrowserWorker`

- `BrowserWorker.layerManager` -> `BrowserWorker.layerPlatform`: WorkerManager was removed. Provide WorkerPlatform directly, or use BrowserWorker.layer(spawn) when a Worker.Spawner is also required.

- `BrowserWorker.layerWorker` -> `BrowserWorker.layerPlatform`: PlatformWorker became Worker.WorkerPlatform. The platform-only layer no longer takes a spawn callback; BrowserWorker.layer(spawn) combines platform and spawner layers.

## `@effect/platform-browser/BrowserWorkerRunner`

- `BrowserWorkerRunner.launch` -> `Layer.launch + RpcServer.layerProtocolWorkerRunner`: The close-latch launcher was removed. Compose BrowserWorkerRunner.layer with the worker RPC server protocol layer and launch the resulting handler layer.

## `@effect/platform-browser/Clipboard`

- `Clipboard.Clipboard` -> `Clipboard.Clipboard`: The service remains, now as a Context.Service with a private brand; normal access and provision are unchanged.

- `Clipboard.ErrorTypeId` -> `none`: The error marker is private in v4; discriminate ClipboardError by its \_tag instead.

- `Clipboard.TypeId` -> `none`: The service brand is private in v4; use the Clipboard Context.Service value.

## `@effect/platform-browser/Geolocation`

- `Geolocation.ErrorTypeId` -> `none`: The error marker is private in v4; discriminate GeolocationError and its tagged reason.

- `Geolocation.Geolocation` -> `Geolocation.Geolocation`: The service remains, now as a Context.Service with a private brand.

- `Geolocation.GeolocationError` -> `Geolocation.GeolocationError`: The class remains, but reason is now PositionUnavailable, PermissionDenied, or Timeout, with the cause stored on that tagged reason.

- `Geolocation.TypeId` -> `none`: The service marker is private in v4; use the Geolocation Context.Service value.

## `@effect/platform-browser/Permissions`

- `Permissions.ErrorTypeId` -> `none`: The error marker is private in v4; discriminate PermissionsError and its tagged reason.

- `Permissions.Permissions` -> `Permissions.Permissions`: The query service remains, now as a Context.Service with a private brand.

- `Permissions.PermissionsError` -> `Permissions.PermissionsError`: The class remains, but reason is now PermissionsInvalidStateError or PermissionsTypeError, with the cause stored on that tagged reason.

- `Permissions.TypeId` -> `none`: The service marker is private in v4; use the Permissions Context.Service value.
