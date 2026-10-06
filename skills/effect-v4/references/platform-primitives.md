# Platform Primitives (FileSystem, Path, Terminal, Stdio, Crypto, ChildProcess, KeyValueStore, Socket)

Use this guide for code that touches the OS: files, paths, the terminal, stdio, processes, randomness, simple storage, or sockets.

## Where things live

| Service / module | Import | Notes |
|---|---|---|
| `FileSystem`, `Path`, `Terminal`, `Stdio`, `Crypto`, `PlatformError` | `import { FileSystem, Path, Terminal, Stdio, Crypto, PlatformError } from "effect"` | Core modules. `Path.layer` is a pure POSIX implementation. `FileSystem.layerNoop(partial)` and `Stdio.layerTest(partial)` exist for tests. |
| `ChildProcess`, `ChildProcessSpawner` | `effect/process` | Replaces v3 `@effect/platform` `Command` / `CommandExecutor`. |
| `KeyValueStore`, `RateLimiter`, `Persistence` | `effect/persistence` | |
| `Socket`, `SocketServer` | `effect/socket` | |
| `NetAddress`, `IpNetwork`, `IpInterface` | `effect/net` | |

Platform implementations come from separate packages:

- `@effect/platform-node`: `NodeServices`, `NodeFileSystem`, `NodePath`, `NodeTerminal`, `NodeStdio`, `NodeCrypto`, `NodeChildProcessSpawner`, `NodeSocket`, `NodeSocketServer`, `NodeRuntime`.
- `@effect/platform-bun`: `BunServices`, `BunFileSystem`, ..., `BunRuntime`.
- `@effect/platform-deno`: `DenoServices`, ...
- `@effect/platform-browser`: `BrowserKeyValueStore`, `BrowserCrypto`, `IndexedDb`, `Clipboard`, `Geolocation`, `Permissions`, ...

**One layer for everything:** `NodeServices.layer` (or `BunServices.layer` / `DenoServices.layer`) provides `ChildProcessSpawner | Crypto | FileSystem | Path | Stdio | Terminal`. It replaces v3 `NodeContext.layer`.

```ts
import { NodeRuntime, NodeServices } from "@effect/platform-node"
program.pipe(Effect.provide(NodeServices.layer), NodeRuntime.runMain)
```

## Errors: `PlatformError`

All platform services fail with `PlatformError` (`_tag: "PlatformError"`), which has a `reason`:

- `BadArgument { module, method, description? }`
- `SystemError { _tag: SystemErrorTag, module, method, syscall?, pathOrDescriptor?, description? }`

`SystemErrorTag` is one of `"AlreadyExists" | "BadResource" | "Busy" | "InvalidData" | "NotFound" | "PermissionDenied" | "TimedOut" | "UnexpectedEof" | "Unknown" | "WouldBlock" | "WriteZero"`. Constructors are `PlatformError.systemError({...})` and `PlatformError.badArgument({...})`.

```ts
const readOrEmpty = (path: string) =>
  FileSystem.FileSystem.use((fs) => fs.readFileString(path)).pipe(
    Effect.catchReason("PlatformError", "NotFound", () => Effect.succeed(""))
  )
```

Map `PlatformError` to domain errors at the adapter boundary. Don't leak it through service interfaces.

## FileSystem and Path

```ts
const program = Effect.gen(function*() {
  const fs = yield* FileSystem.FileSystem
  const path = yield* Path.Path
  const dir = yield* fs.makeTempDirectoryScoped()       // removed when the scope closes
  const file = path.join(dir, "data.json")
  yield* fs.writeFileString(file, JSON.stringify({ ok: true }))
  const text = yield* fs.readFileString(file)
  const exists = yield* fs.exists(file)
  const info = yield* fs.stat(file)                     // info.size is a ByteSize
  yield* fs.makeDirectory(path.join(dir, "a/b"), { recursive: true })
  const tsFiles = yield* fs.glob("**/*.ts", { root: dir, exclude: ["node_modules/**"] })
  yield* fs.stream(file, { chunkSize: 64 * 1024 }).pipe(Stream.runDrain)
  yield* fs.remove(dir, { recursive: true, force: true })
}).pipe(Effect.scoped)
```

- `FileSystem` methods: `access`, `copy`, `copyFile`, `chmod`, `chown`, `exists`, `glob`, `link`, `makeDirectory`, `makeTempDirectory(Scoped)`, `makeTempFile(Scoped)`, `open` (scoped `File` handle), `readDirectory`, `readFile`, `readFileString`, `readLink`, `realPath`, `remove`, `rename`, `sink`, `stat`, `stream`, `symlink`, `truncate`, `utimes`, `watch` (a `Stream<WatchEvent>`), `writeFile`, `writeFileString`.
- `Path` methods: `join`, `resolve`, `relative`, `normalize`, `dirname`, `basename`, `extname`, `parse`, `format`, `isAbsolute`, `sep`, `fromFileUrl`, `toFileUrl`, `toNamespacedPath`. Never build paths by string concatenation.

## Terminal and Stdio

- `Terminal.Terminal` (interactive TTY):
  - `columns`, `rows`
  - `readLine` (fails with `Terminal.QuitError` on Ctrl+C/D)
  - `readInput` (scoped `Queue.Dequeue<UserInput>` of key presses)
  - `display(text)`
- `Stdio.Stdio` (raw process streams, new in v4):
  - `args`, `stdinIsTerminal`, `stdoutIsTerminal`
  - `stdin`, a `Stream<Uint8Array>`
  - `stdout(options?)` and `stderr(options?)` return a `Sink<void, string | Uint8Array>`
- `effect/cli` uses `Stdio.args`. Tests can provide `Stdio.layerTest({ args: Effect.succeed([...]) })`.

## Crypto (new in v4)

```ts
const ids = Effect.gen(function*() {
  const crypto = yield* Crypto.Crypto
  const id = yield* crypto.randomUUIDv7          // also randomUUIDv4, randomULID
  const bytes = yield* crypto.randomBytes(32)
  const hash = yield* crypto.digest("SHA-256", bytes)  // "SHA-1" | "SHA-256" | "SHA-384" | "SHA-512"
  const n = yield* crypto.randomIntBetween(1, 6)
})
```

Every platform package provides `Crypto`. Use `Crypto.make({ randomBytes, digest })` for deterministic test layers.

## Child processes (`effect/process`)

Official example: `references/examples/60_child-process/10_working-with-child-processes.ts`.

```ts
import { ChildProcess, ChildProcessSpawner } from "effect/process"

const program = Effect.gen(function*() {
  const spawner = yield* ChildProcessSpawner.ChildProcessSpawner

  const version = yield* spawner.string(ChildProcess.make("node", ["--version"]))   // whole stdout
  const files = yield* spawner.lines(ChildProcess.make`git diff --name-only HEAD~1`) // template form
  const code = yield* spawner.exitCode(ChildProcess.make("make", ["test"], { cwd: "/repo" }))

  // pipelines
  const top = yield* spawner.lines(
    ChildProcess.make("git", ["log", "--oneline"]).pipe(ChildProcess.pipeTo(ChildProcess.make("head", ["-n", "5"])))
  )

  // long-running: get a handle (needs a Scope); the process is killed when the scope closes
  yield* Effect.scoped(Effect.gen(function*() {
    const handle = yield* spawner.spawn(ChildProcess.make("pnpm", ["build"], { env: { CI: "1" }, extendEnv: true }))
    // or simply: const handle = yield* ChildProcess.make("pnpm", ["build"])  (commands are Effects)
    yield* handle.all.pipe(Stream.decodeText(), Stream.splitLines, Stream.runForEach(Console.log))
    const exit = yield* handle.exitCode
    if (exit !== ChildProcessSpawner.ExitCode(0)) return yield* new BuildFailed({ exit })  // a Schema.TaggedError
  }))
})
```

- `ChildProcess.make` has three forms:
  - `make("cmd", [args], options?)`
  - `` make`cmd args` ``
  - `` make({ cwd })`cmd args` ``
- `CommandOptions`: `cwd`, `env`, `extendEnv`, `shell`, `detached`, `windowsHide`, `stdin` (e.g. a `Stream` to feed input), `stdout`, `stderr`, `additionalFds`.
- Combinators: `ChildProcess.setCwd`, `ChildProcess.setEnv`, `ChildProcess.prefix`, `ChildProcess.pipeTo`.
- Spawner methods: `spawn`, `exitCode`, `string`, `lines`, `streamString`, `streamLines`. The collectors take `{ includeStderr }`.
- `ChildProcessHandle` has:
  - `pid`, `exitCode`, `isRunning`
  - `kill({ killSignal?, forceKillAfter? })`
  - `stdin` (a `Sink`), `stdout`, `stderr`, `all` (both outputs merged)
  - `getInputFd(n)`, `getOutputFd(n)`
  - `unref`, which lets the runtime exit while the child keeps running and returns a `reref` effect
- Always bound long commands with `Effect.timeout`. Interruption kills the process.

## KeyValueStore (`effect/persistence`)

```ts
import { KeyValueStore } from "effect/persistence"

const program = Effect.gen(function*() {
  const kv = yield* KeyValueStore.KeyValueStore
  yield* kv.set("token", "abc")
  const token = yield* kv.get("token")                    // string | undefined
  const users = KeyValueStore.toSchemaStore(kv, User)     // typed JSON get/set via Schema
  const scoped = KeyValueStore.prefix(kv, "cache:")
})
```

- Store methods: `get`, `getUint8Array`, `set`, `remove`, `clear`, `size`, `modify`, `has`, `isEmpty`.
- Layers:
  - `KeyValueStore.layerMemory`
  - `KeyValueStore.layerFileSystem(dir)` (needs `FileSystem | Path`)
  - `KeyValueStore.layerSql({ table? })` (needs `SqlClient`)
  - `KeyValueStore.layerStorage(() => storage)` (any Web `Storage`)
  - Browser: `BrowserKeyValueStore.layerLocalStorage`, `layerSessionStorage`, `layerIndexedDb()`
  - Deno: `DenoKeyValueStore.layerLocalStorage` / `layerSessionStorage`
- Errors are `KeyValueStoreError`.

## Sockets and networking (brief)

- `Socket.Socket` uses a pull-based reader with backpressure:
  - `yield* socket.reader` gives `{ pull, upgrade }`, where `upgrade` does STARTTLS.
  - `yield* socket.writer` gives `{ write, writeAll }`.
  - Both need a `Scope`.
  - Helpers: `Socket.readerString`, `Socket.toChannel`.
- Clients:
  - `Socket.makeWebSocket(url)` / `Socket.layerWebSocket(url)` need a `WebSocketConstructor`, from `NodeSocket.layerWebSocketConstructor` or `Socket.layerWebSocketConstructorGlobal`.
  - TCP: `NodeSocket.makeNet` / `layerNet`. TLS: `makeTls` / `layerTls`.
- Servers: `NodeSocketServer.layer` / `layerTls` / `layerWebSocket`.
- `effect/net` models addresses (`NetAddress.ipFromString`, `inetAddress`, `formatUrl`, ...) and CIDRs (`IpNetwork.fromString(...)`, `contains`).

## Pitfalls

- Missing a platform layer shows up as an unsatisfied `FileSystem | Path | ...` requirement. Provide `NodeServices.layer` at the edge, or inside a service's own layer, as the official examples do.
- Using `node:fs` / `child_process` directly. You lose interruption, scoping, and testability.
- Forgetting `Effect.scoped` around `spawn`, `open`, `makeTemp*Scoped`, or socket readers.

## v3 → v4

| v3 | v4 |
|---|---|
| `@effect/platform` `Command` / `CommandExecutor` | `effect/process` `ChildProcess` / `ChildProcessSpawner` |
| `NodeContext.layer` | `NodeServices.layer` |
| `@effect/platform/KeyValueStore` | `effect/persistence/KeyValueStore` |
| `@effect/platform/FileSystem` / `Path` / `Terminal` | core `effect` modules |

See `references/migration/api-map/platform.md`, `platform-node.md`, and `platform-bun.md`.
