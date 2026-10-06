# v3 → v4 API map: @effect/platform-node-shared (6 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/platform-node-shared/NodeCommandExecutor`

- `NodeCommandExecutor.layer` -> `NodeChildProcessSpawner.layer`: CommandExecutor became effect/process/ChildProcessSpawner; the Node adapter was renamed and still requires FileSystem and Path.

## `@effect/platform-node-shared/NodeFileSystem/ParcelWatcher`

- `ParcelWatcher.layer` -> `NodeFileSystem.layer`: The Parcel watcher adapter was removed. NodeFileSystem.layer uses node:fs.watch; provide a custom FileSystem.WatchBackend for specialized behavior.

## `@effect/platform-node-shared/NodeMultipart`

- `NodeMultipart.fileToReadable` -> `@effect/platform-node/NodeMultipart#fileToReadable`: The Node multipart implementation moved from @effect/platform-node-shared to @effect/platform-node; its behavior remains.

- `NodeMultipart.stream` -> `@effect/platform-node/NodeMultipart#stream`: The Node multipart implementation moved from @effect/platform-node-shared to @effect/platform-node; the source and headers call shape remains.

## `@effect/platform-node-shared/NodeSink`

- `NodeSink.stderr` -> `stdio.stderr() from Stdio.Stdio`: Standard error moved to effect/Stdio; provide NodeStdio.layer or NodeServices.layer.

- `NodeSink.stdin` -> `NodeSink.fromWritable({ evaluate: () => process.stdin, onError: ... })`: There is no Stdio sink because stdin is a readable stream in v4; use a manual adapter only if writing to process.stdin was intentional.

- `NodeSink.stdout` -> `stdio.stdout() from Stdio.Stdio`: Standard output moved to effect/Stdio; provide NodeStdio.layer or NodeServices.layer.

## `@effect/platform-node-shared/NodeSocket`

- `NodeSocket.NetSocket` -> `NodeSocket.NetSocket`: The identifier remains, but the old interface/tag pair is now one Context.Service; use NodeSocket.NetSocket["Service"] for the node:net.Socket value type.

## `@effect/platform-node-shared/NodeStream`

- `NodeStream.FromReadableOptions` -> `{ readonly chunkSize?: number; readonly closeOnDone?: boolean }`: The named interface was removed and its fields were inlined into readable constructor options; chunkSize narrowed from SizeInput to number.

- `NodeStream.FromWritableOptions` -> `{ readonly endOnDone?: boolean; readonly encoding?: BufferEncoding }`: The named interface was removed and its fields were inlined into NodeSink and duplex constructor options.

- `NodeStream.stderr` -> `NodeStream.fromReadable({ evaluate: () => process.stderr, closeOnDone: false }).pipe(Stream.orDie)`: This preserves the unusual v3 read behavior; for normal error output use the stdio.stderr() Sink from effect/Stdio.

- `NodeStream.stdin` -> `stdio.stdin from Stdio.Stdio`: Standard input moved to effect/Stdio; provide NodeStdio.layer or NodeServices.layer. The service stream exposes PlatformError instead of dying.

- `NodeStream.stdout` -> `NodeStream.fromReadable({ evaluate: () => process.stdout, closeOnDone: false }).pipe(Stream.orDie)`: This preserves the unusual v3 read behavior; for normal output use the stdio.stdout() Sink from effect/Stdio.
