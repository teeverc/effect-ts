# CLI applications (`effect/cli`)

Use this guide to build command-line tools with typed arguments, flags, subcommands, prompts, completions, and wizard mode. Official example: `references/examples/70_cli/10_basics.ts`.

```ts
import { Argument, CliConfig, CliError, CliOutput, Command, Flag, GlobalFlag, Prompt } from "effect/cli"
import { NodeRuntime, NodeServices } from "@effect/platform-node"
```

`@effect/cli` was merged into `effect` (`effect/cli`). The modules are `@stability unstable`.

## Mental model

- `Command.make(name, config?, handler?)` declares a command. `config` is a record of `Flag`s and `Argument`s, and can be nested. The handler receives the parsed, typed values.
- Flags and arguments parse strings. You refine them with `withSchema`, `map`, `filter`, `withDefault`, and `optional`.
- Commands form a tree via `Command.withSubcommands`. A subcommand handler can `yield*` a **parent command** to read the parent's parsed input, including shared flags.
- `Command.run(cmd, { version })` reads `Stdio.args`, parses, renders help and errors, and runs the handler. It requires `Command.Environment = FileSystem | Path | Terminal | ChildProcessSpawner | Stdio`, which `NodeServices.layer` / `BunServices.layer` provide.

## Example

```ts
import { Console, Effect, Option, Schema } from "effect"

const workspace = Flag.String("workspace").pipe(
  Flag.withAlias("w"),
  Flag.withDescription("Workspace to operate on"),
  Flag.withDefault("personal")
)

const tasks = Command.make("tasks").pipe(
  Command.withSharedFlags({ workspace }),              // available to this command and its subcommands
  Command.withDescription("Track and manage tasks")
)

const create = Command.make(
  "create",
  {
    title: Argument.String("title").pipe(Argument.withSchema(Schema.NonEmptyString)),
    priority: Flag.Literals("priority", ["low", "normal", "high"]).pipe(Flag.withDefault("normal")),
    assignee: Flag.String("assignee").pipe(Flag.optional)   // Option<string>
  },
  Effect.fn(function*({ title, priority, assignee }) {
    const root = yield* tasks                          // parent input: { workspace }
    yield* Console.log(`Created "${title}" in ${root.workspace} (${priority})`)
    if (Option.isSome(assignee)) yield* Console.log(`Assigned to ${assignee.value}`)
  })
).pipe(
  Command.withDescription("Create a task"),
  Command.withExamples([{ command: `tasks create "Ship 4.0" --priority high`, description: "High priority" }])
)

const list = Command.make("list", { json: Flag.Boolean("json").pipe(Flag.withDefault(false)) }, ({ json }) =>
  Console.log(json ? "[]" : "no tasks")
).pipe(Command.withAlias("ls"))

tasks.pipe(
  Command.withSubcommands([create, list]),
  Command.run({ version: "1.0.0" }),
  Effect.provide(NodeServices.layer),
  NodeRuntime.runMain
)
```

## Flags and arguments

- Constructors. Flags and arguments take the name as the first argument.
  - Shared by `Flag` and `Argument`: `String`, `Int`, `Finite`, `Date`, `Literals(name, [...])`, `ChoiceWithValue(name, [[label, value], ...])`, `Path(name, { pathType?, mustExist? })`, `File`, `Directory`, `Redacted`, `FileText`, `FileParse`, `FileSchema(name, schema)`.
  - Flag only: `Flag.Boolean`, `Flag.KeyValuePair` (`--env k=v`, giving a `Record<string, string>`).
- Combinators. Every one listed here exists on both `Flag` and `Argument`; `withAlias` and `withHidden` are `Flag`-only.
  - `withDescription`, `withMetavar`, `withAlias("x")`, `withHidden`
  - `withDefault(v)`, `optional` (gives `Option<A>`)
  - `withFallbackConfig(Config.String("ENV_VAR"))`, `withFallbackPrompt(Prompt.String({ message }))`
  - `withSchema(schema)`, `map`, `mapEffect`, `mapTryCatch`, `filter`, `filterMap`, `orElse`, `orElseResult`
  - `atLeast(n)`, `atMost(n)`, `between(min, max)`
- Repeatables: `Argument.variadic({ min?, max? })` gives a `ReadonlyArray<A>`. `Flag.atLeast` / `atMost` / `between` allow a flag to repeat.
- There is no `Args.all` / `Options.all`. Put params in the config record; nested records are allowed.

## Commands

- `Command.make(name)` creates a container with no handler. Add one with `Command.withHandler(fn)`.
- Configuration: `withDescription`, `withShortDescription` (shown in subcommand lists), `withAlias("ls")`, `withExamples([...])`, `Command.unlisted` (hidden from help), `annotate` / `annotateMerge`.
- Subcommands: `withSubcommands([cmdA, { group: "Auth commands", commands: [login, logout] }])`. The group label appears in help.
- `withSharedFlags({...})` is parent-level input that subcommands read by yielding the parent.
- `withGlobalFlags([...])` makes flags apply to the command and all descendants at any position (see below).
- Services:
  - `Command.provide(layer | (input) => layer)`, `provideSync`, `provideEffect`, `provideEffectDiscard`.
  - The input-dependent form lets you choose implementations from flags, e.g. `--env local`.

## Global flags (new in v4)

```ts
const Region = GlobalFlag.Setting("region")({ flag: Flag.String("region").pipe(Flag.withDefault("us-west-2")) })
const cli = Command.make("deploy", {}, () => Effect.gen(function*() {
  const region = yield* Region                       // a Setting is a Context service
})).pipe(Command.withGlobalFlags([Region]))

const Debug = GlobalFlag.Action({                    // an Action runs and then exits (like --help)
  flag: Flag.Boolean("print-config").pipe(Flag.withDefault(false)),
  run: (enabled, { command, commandPath }) => enabled ? Console.log(commandPath.join(" ")) : Effect.void
})
```

Built-ins (`GlobalFlag.BuiltIns`):

- `Help` (`--help` / `-h`)
- `Version` (`--version` / `-v`)
- `Wizard` (`--wizard`)
- `Completions` (`--completions bash|zsh|fish|sh`), which prints a shell completion script
- `LogLevel` (`--log-level <level>`)

Customise the set with `CliConfig.layer({ builtIns: [...] })`.

## Wizard mode and prompts

- `mycli --wizard` (or `Command.wizard(cmd)`) interactively walks the command tree and prompts for every flag and argument. It returns the assembled argv.
- `Prompt` values are Effects requiring `FileSystem | Path | Terminal`. Run them with `yield* prompt` or `Prompt.run(prompt)`.
  - Constructors: `Prompt.String({ message, default?, validate? })`, `Password` (gives `Redacted`), `Hidden`, `Int`, `Number`, `Confirm({ message, initial? })`, `Toggle`, `Date`, `List`, `File`, `Select({ message, choices: [{ title, value, description? }] })`, `MultiSelect`, `AutoComplete`.
  - Combinators: `Prompt.all({...} | [...])`, `map`, `flatMap`, `succeed`, `Custom`.
  - Ctrl+C fails with `Terminal.QuitError`.

## Output, errors, testing

- `CliError` covers `UnrecognizedOption`, `DuplicateOption`, `MissingOption`, `MissingArgument`, `UnexpectedArgument`, `InvalidValue`, `UnknownSubcommand`, `UserError`, and `ShowHelp`. `Command.run` renders them; use the `renderErrors` option to control this. Errors from your handler stay in the effect's error channel (`E | CliError`).
- `CliOutput.layer(formatter)` replaces help, version, and error rendering. `CliOutput.defaultFormatter` is the starting point.
- Test with `Command.runWith(cmd, { version })(["create", "x", "--priority", "high"])`. Provide a test environment:

  ```ts
  Layer.mergeAll(
    FileSystem.layerNoop({}),
    Path.layer,
    Stdio.layerTest({}),
    Layer.succeed(Terminal.Terminal, Terminal.make({...})),
    Layer.succeed(ChildProcessSpawner.ChildProcessSpawner, ChildProcessSpawner.make(() => Effect.die("unused")))
  )
  ```

  `Console` can be captured with `Effect.provideService(Console.Console, ...)`.

## v3 → v4

| v3 (`@effect/cli`) | v4 (`effect/cli`) |
|---|---|
| `Args` | `Argument`, with the name as the first parameter: `Args.text({ name })` → `Argument.String("name")` |
| `Options` | `Flag`: `Options.text("x")` → `Flag.String("x")`, `Options.choice` → `Flag.Literals` |
| `Command.run(cmd, { name, version })(process.argv)` | `Command.run(cmd, { version })`, which reads argv from `Stdio` |
| `NodeContext.layer` | `NodeServices.layer` |
| parent options visible to subcommands | declare them with `Command.withSharedFlags` and read them by yielding the parent command |

Full table: `references/migration/api-map/cli.md`.
