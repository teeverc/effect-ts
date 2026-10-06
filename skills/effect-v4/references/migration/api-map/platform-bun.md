# v3 → v4 API map: @effect/platform-bun (11 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/platform-bun/BunCommandExecutor`

- `BunCommandExecutor.layer` -> `BunChildProcessSpawner.layer`: CommandExecutor became effect/process/ChildProcessSpawner; the Bun adapter was renamed and still requires FileSystem and Path.

## `@effect/platform-bun/BunContext`

- `BunContext.BunContext` -> `BunServices.BunServices`: The aggregate was renamed and now provides ChildProcessSpawner, Crypto, FileSystem, Path, Stdio, and Terminal; add BunWorker separately when needed.

- `BunContext.layer` -> `BunServices.layer`: Use the renamed aggregate layer; worker services are no longer included.

## `@effect/platform-bun/BunFileSystem/ParcelWatcher`

- `ParcelWatcher.layer` -> `BunFileSystem.layer`: The Parcel watcher adapter was removed. BunFileSystem.layer uses the built-in node:fs-compatible watcher; provide a custom FileSystem.WatchBackend for specialized behavior.

## `@effect/platform-bun/BunHttpPlatform`

- `BunHttpPlatform.make` -> `BunHttpPlatform.layer`: The Bun-specific constructor is private; provide the public layer and consume HttpPlatform.HttpPlatform.

## `@effect/platform-bun/BunHttpServer`

- `BunHttpServer.ServeOptions` -> `BunHttpServer.ServeOptions<R>`: The alias remains, but R is now a route-key string union and routes uses Bun.Serve.Routes; update old route-map generic arguments or infer R from routes.

- `BunHttpServer.layerContext` -> `BunHttpServer.layerHttpServices`: Direct rename; it provides HttpPlatform, Etag.Generator, and BunServices.

## `@effect/platform-bun/BunHttpServerRequest`

- `BunHttpServerRequest.toRequest` -> `BunHttpServerRequest.toBunServerRequest`: Direct rename with the more precise Bun.BunRequest result type.

## `@effect/platform-bun/BunSink`

- `BunSink.stderr` -> `stdio.stderr() from Stdio.Stdio`: Process stdio moved behind effect/Stdio; provide BunStdio.layer. stderr remains a Sink and can be configured with endOnDone.

- `BunSink.stdin` -> `stdio.stdin from Stdio.Stdio`: Standard input is correctly modeled as a Stream in v4, not a writable Sink; manually adapt process.stdin only if writing to it was intentional.

- `BunSink.stdout` -> `stdio.stdout() from Stdio.Stdio`: Process stdio moved behind effect/Stdio; provide BunStdio.layer. stdout remains a Sink.

## `@effect/platform-bun/BunSocket`

- `BunSocket.NetSocket` -> `BunSocket.NetSocket`: The identifier remains, but the old interface/tag pair is now one Context.Service for node:net.Socket.

## `@effect/platform-bun/BunSocketServer`

- `BunSocketServer.IncomingMessage` -> `BunSocketServer.IncomingMessage`: The identifier remains and is now a Context.Service for node:http.IncomingMessage.

## `@effect/platform-bun/BunStream`

- `BunStream.FromReadableOptions` -> `Pick<Parameters<typeof BunStream.fromReadable>[0], "chunkSize" | "closeOnDone">`: The named interface was inlined into the constructor options; chunkSize is now a number and the full options also contain evaluate and onError. The ignored bufferSize option was removed.

- `BunStream.FromWritableOptions` -> `Pick<Parameters<typeof BunSink.fromWritable>[0], "endOnDone" | "encoding">`: The named interface was inlined into BunSink.fromWritable and duplex constructor options.

- `BunStream.stderr` -> `stdio.stderr() from Stdio.Stdio`: Standard error is correctly modeled as a Sink in v4. Explicitly adapt process.stderr with BunStream.fromReadable only to preserve the old unusual read behavior.

- `BunStream.stdin` -> `stdio.stdin from Stdio.Stdio`: Standard input moved to the Stdio service; provide BunStdio.layer. Its stream exposes PlatformError instead of dying.

- `BunStream.stdout` -> `stdio.stdout() from Stdio.Stdio`: Standard output is correctly modeled as a Sink in v4. Explicitly adapt process.stdout with BunStream.fromReadable only to preserve the old unusual read behavior.

## `@effect/platform-bun/BunWorker`

- `BunWorker.layerManager` -> `BunWorker.layerPlatform`: WorkerManager was removed. Provide WorkerPlatform directly, or use BunWorker.layer(spawn) when a Worker.Spawner is also required.

- `BunWorker.layerWorker` -> `BunWorker.layerPlatform`: PlatformWorker became Worker.WorkerPlatform; BunWorker.layer(spawn) combines the platform and spawner layers.
