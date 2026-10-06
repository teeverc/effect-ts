# v3 → v4 API map: @effect/cli (13 modules)

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `@effect/cli/Args`

- `Args.All.ArgsAny` -> `Param.AnyArgument`: Use the shared any-positional-parameter type.

- `Args.All.Return` -> `Command.Command.Config.Infer`: Infer the output of a command config record; standalone argument collections were removed.

- `Args.Args` -> `Argument.Argument`: Args was renamed to Argument in effect/cli.

- `Args.Args.BaseArgsConfig` -> `name: string`: Argument constructors now take the name as a required first parameter.

- `Args.Args.FormatArgsConfig` -> `Primitive.FileParseOptions`: Pass the name separately and use the format option with Argument.FileParse or Argument.FileSchema.

- `Args.Args.PathArgsConfig` -> `Argument.Path(name, { pathType, mustExist })`: Path options are inline; map exists=yes to mustExist=true and either to omission. exists=no has no exact replacement.

- `Args.Args.Variance` -> `Argument.Argument`: The separate variance artifact was removed; Argument inherits the shared Param variance.

- `Args.ArgsTypeId` -> `Param.isParam(value) && value.kind === Param.argumentKind`: The public Args type id was removed; use the Param guard and argument kind discriminator.

- `Args.all` -> `Command.make(name, config)`: Collect arguments in the config record passed to Command.make; there is no standalone Argument.all.

- `Args.atLeast` -> `Argument.atLeast`: Use the moved combinator; v4 returns ReadonlyArray and does not encode non-emptiness in the type.

- `Args.atMost` -> `Argument.atMost`: Use the moved combinator.

- `Args.between` -> `Argument.between`: Use the moved combinator; v4 validates bounds when constructing the parameter.

- `Args.boolean` -> `Flag.Boolean / Argument.ChoiceWithValue`: Positional booleans were removed as ambiguous; prefer a boolean flag or explicit true/false positional choices.

- `Args.choice` -> `Argument.Literals`: Use the renamed constructor and pass the argument name explicitly.

- `Args.date` -> `Argument.Date`: Use the renamed constructor and pass the argument name explicitly.

- `Args.directory` -> `Argument.Directory`: Use mustExist=true for exists=yes and omit it for either; exists=no has no exact replacement.

- `Args.file` -> `Argument.File`: Use mustExist=true for exists=yes and omit it for either; exists=no has no exact replacement.

- `Args.fileContent` -> `Argument.File + Argument.mapEffect`: Parse a path and read it with FileSystem.readFile; no binary-content argument constructor remains.

- `Args.fileParse` -> `Argument.FileParse`: Pass the old format as an options field; v4 returns parsed content rather than a path/content tuple.

- `Args.fileSchema` -> `Argument.FileSchema`: Pass the old format as an options field and use a v4 Schema constraint decoder.

- `Args.fileText` -> `Argument.FileText`: Use the renamed constructor; it returns content only.

- `Args.float` -> `Argument.Finite`: Use the renamed constructor; it rejects non-finite numbers.

- `Args.getHelp` -> `none`: Per-argument help introspection was removed; Command generates help internally.

- `Args.getIdentifier` -> `none`: Public argument identifier introspection was removed.

- `Args.getMaxSize` -> `none`: Public arity introspection was removed; command parsing enforces variadic bounds internally.

- `Args.getMinSize` -> `none`: Public arity introspection was removed; command parsing enforces variadic bounds internally.

- `Args.getUsage` -> `none`: The public Usage tree was removed; Command generates a usage string internally.

- `Args.integer` -> `Argument.Int`: Use the renamed constructor and pass the argument name explicitly.

- `Args.isArgs` -> `Param.isParam(value) && value.kind === Param.argumentKind`: Arguments now use the shared Param representation and an explicit kind discriminator.

- `Args.map` -> `Argument.map`: Use the moved combinator.

- `Args.mapEffect`: TODO: needs guidance

- `Args.none` -> `omit the config entry`: V4 Argument.Never is an always-failing sentinel, not v3's empty successful argument set.

- `Args.optional` -> `Argument.optional`: Use the moved combinator; it still returns Option.

- `Args.path` -> `Argument.Path`: Path options are inline; map exists=yes to mustExist=true and either to omission. exists=no has no exact replacement.

- `Args.redacted` -> `Argument.Redacted`: Use the renamed constructor and pass the argument name explicitly.

- `Args.repeated` -> `Argument.variadic`: Renamed to variadic; pass optional min and max bounds.

- `Args.secret` -> `Argument.Redacted`: Use Redacted-backed positional input.

- `Args.text` -> `Argument.String`: Renamed to String; pass the argument name explicitly.

- `Args.validate` -> `argument.parse({ flags: {}, arguments: args })`: Parsing is now a Param method and returns leftover tokens with the value; errors are CliError.

- `Args.withDefault` -> `Argument.withDefault`: Use the moved combinator; v4 also accepts an Effect fallback.

- `Args.withDescription` -> `Argument.withDescription`: Use the moved combinator.

- `Args.withFallbackConfig` -> `Argument.withFallbackConfig`: Use the moved combinator; invalid configuration becomes CliError.InvalidValue.

- `Args.withSchema` -> `Argument.withSchema`: Use the moved combinator with a v4 Schema constraint decoder.

## `@effect/cli/BuiltInOptions`

- `BuiltInOptions.BuiltInOptions` -> `GlobalFlag.BuiltIn`: The parsed directive union became a union of global Action and Setting definitions.

- `BuiltInOptions.BuiltInOptions.ShellType` -> `Completions.Shell`: The shell union moved to Completions.

- `BuiltInOptions.SetLogLevel` -> `GlobalFlag.LogLevel`: Log level is now a global Setting whose parsed value is provided through context.

- `BuiltInOptions.ShowCompletions` -> `GlobalFlag.Completions`: The directive payload was replaced by a global completion action definition.

- `BuiltInOptions.ShowHelp` -> `GlobalFlag.Help`: The directive payload was replaced by a global help action definition.

- `BuiltInOptions.ShowVersion` -> `GlobalFlag.Version`: The directive value was replaced by a global version action definition.

- `BuiltInOptions.ShowWizard` -> `GlobalFlag.Wizard`: The directive payload was replaced by a global wizard action definition.

- `BuiltInOptions.builtInOptions` -> `GlobalFlag.BuiltIns`: Built-ins are global flag definitions consumed automatically by Command.run and Command.runWith.

- `BuiltInOptions.isShowCompletions` -> `none`: Parsed ShowCompletions directives were removed; the runner processes GlobalFlag.Completions directly.

- `BuiltInOptions.isShowHelp` -> `none`: Parsed ShowHelp directives were removed; the runner processes GlobalFlag.Help directly.

- `BuiltInOptions.isShowVersion` -> `none`: Parsed ShowVersion directives were removed; the runner processes GlobalFlag.Version directly.

- `BuiltInOptions.isShowWizard` -> `none`: Parsed ShowWizard directives were removed; the runner processes GlobalFlag.Wizard directly.

- `BuiltInOptions.showCompletions` -> `GlobalFlag.Completions`: Use the built-in completion action; the shell is parsed from --completions.

- `BuiltInOptions.showHelp` -> `GlobalFlag.Help`: Use the built-in help action; usage and help are derived from the active Command.

- `BuiltInOptions.showVersion` -> `GlobalFlag.Version`: Use the built-in version action; supply the version to Command.run or Command.runWith.

- `BuiltInOptions.showWizard` -> `GlobalFlag.Wizard`: Use the built-in wizard action; runner context supplies the active Command.

## `@effect/cli/CliApp`

- `CliApp.CliApp` -> `Command.Command`: The separate application wrapper was folded into the runnable v4 Command tree.

- `CliApp.CliApp.ConstructorArgs` -> `none`: Build the command with Command.make and withDescription, then pass version to Command.run; the old app constructor shape was removed.

- `CliApp.make` -> `Command.make`: Build the executable Command directly; there is no separate CliApp wrapper.

- `CliApp.run` -> `Command.run`: The CliApp wrapper was removed. Attach the execute function with Command.withHandler, then run the Command with its version; v4 reads arguments through the CLI environment instead of accepting args and execute at this call.

## `@effect/cli/CliConfig`

- `CliConfig.CliConfig` -> `CliConfig.CliConfig.Service`: The service was redesigned to configure built-in global flags; old parser and help switches were removed.

- `CliConfig.defaultConfig` -> `CliConfig.defaults`: Renamed to defaults with the redesigned service shape.

- `CliConfig.defaultLayer` -> `CliConfig.layer`: Call CliConfig.layer() to provide the defaults.

- `CliConfig.layer` -> `CliConfig.layer`: The layer constructor remains, but its options configure the redesigned CliConfig.Service for built-in global flags.

- `CliConfig.make` -> `CliConfig.make`: The constructor remains but accepts the redesigned service options.

- `CliConfig.normalizeCase` -> `none`: Case normalization is no longer configurable through CliConfig.

## `@effect/cli/Command`

- `Command.Command.Config`: TODO: needs guidance

- `Command.Command.Context` -> `Command.CommandContext`: Renamed to CommandContext.

- `Command.Command.ParseConfig` -> `Command.Command.Config.Infer`: Use the v4 command-config inference helper.

- `Command.Command.ParseConfigValue` -> `Command.Command.Config.InferValue`: Use the v4 command-config value inference helper.

- `Command.Command.ParsedConfig` -> `none`: The parsed config representation is internal in v4.

- `Command.Command.ParsedConfigNode` -> `none`: The parsed config node representation is internal in v4.

- `Command.Command.ParsedConfigTree` -> `none`: The parsed config tree representation is internal in v4.

- `Command.Command.Transform` -> `none`: The handler transformation type and machinery are internal in v4.

- `Command.TypeId` -> `Command.isCommand`: The type id is internal in v4; use the public runtime guard.

- `Command.fromDescriptor` -> `Command.make`: The descriptor layer was folded into Command; define config and handler directly on Command.make.

- `Command.getBashCompletions` -> `Completions.generate`: Generation now returns one script string; normally use GlobalFlag.Completions through the runner.

- `Command.getFishCompletions` -> `Completions.generate`: Generation now returns one script string; normally use GlobalFlag.Completions through the runner.

- `Command.getHelp` -> `none`: Help generation for a command path is internal; use GlobalFlag.Help through Command.run or runWith.

- `Command.getNames` -> `Command.Command.name / Command.Command.alias`: Read the public name and optional alias fields; no HashSet accessor remains.

- `Command.getSubcommands` -> `Command.Command.subcommands`: Read the public grouped subcommands field; its shape is no longer a name map.

- `Command.getUsage` -> `none`: Usage is generated internally as part of structured HelpDoc.

- `Command.getZshCompletions` -> `Completions.generate`: Generation now returns one script string; normally use GlobalFlag.Completions through the runner.

- `Command.make` -> `Command.make`: Use the redesigned constructor with one nested config object of Argument and Flag values.

- `Command.run` -> `Command.runWith`: Use runWith for the v3-style function that accepts an argv array; use run to read arguments from Stdio.

- `Command.transformHandler` -> `none`: Transform in the handler or use the specific provide combinators; the generic handler transform is internal.

- `Command.withDescription` -> `Command.withDescription`: Use the retained combinator; v4 descriptions are strings rather than the old HelpDoc ADT.

## `@effect/cli/CommandDescriptor`

- `CommandDescriptor.Command.ComputeParsedType` -> `Types.Simplify`: Use the general simplification utility, or Command.Command.Config.Infer for command config.

- `CommandDescriptor.Command.GetParsedType` -> `none`: No public parsed-input extractor remains; v4 handlers receive inferred config directly.

- `CommandDescriptor.Command.ParsedStandardCommand` -> `none`: The name/options/args parsed wrapper was removed; handlers receive inferred config directly.

- `CommandDescriptor.Command.ParsedUserInputCommand` -> `none`: The descriptor-level prompt command was removed; use Prompt APIs or Command.wizard.

- `CommandDescriptor.Command.Subcommands` -> `none`: Compose independently handled commands with Command.withSubcommands instead of parsing a tuple union.

- `CommandDescriptor.Command.Variance` -> `Command.Command.Variance`: The command variance helper remains conceptually, now tracking input, error, and requirements.

- `CommandDescriptor.TypeId` -> `Command.isCommand`: The type id is internal in v4; use the public runtime guard.

- `CommandDescriptor.getBashCompletions` -> `Completions.generate`: Completion generation moved to one shell-parameterized function; command conversion is internal.

- `CommandDescriptor.getFishCompletions` -> `Completions.generate`: Completion generation moved to one shell-parameterized function; command conversion is internal.

- `CommandDescriptor.getHelp` -> `none`: Help generation is internal to the Command runner.

- `CommandDescriptor.getNames` -> `Command.Command.name / Command.Command.alias`: Read the public fields; no HashSet accessor remains.

- `CommandDescriptor.getSubcommands` -> `Command.Command.subcommands`: Read the public grouped subcommands field.

- `CommandDescriptor.getUsage` -> `none`: Usage generation is internal to Command help generation.

- `CommandDescriptor.getZshCompletions` -> `Completions.generate`: Completion generation moved to one shell-parameterized function; command conversion is internal.

- `CommandDescriptor.make` -> `Command.make`: The descriptor and executable command layers were merged into one constructor.

- `CommandDescriptor.map` -> `none`: Map individual Argument or Flag values, or transform inside the command handler.

- `CommandDescriptor.mapEffect` -> `none`: Use parameter mapEffect where the transformation belongs to an input, or perform the Effect in the handler.

- `CommandDescriptor.parse` -> `Command.runWith`: Parsing was folded into execution and no intermediate CommandDirective is returned.

- `CommandDescriptor.withDescription` -> `Command.withDescription`: Use the retained behavior; v4 descriptions are strings.

## `@effect/cli/CommandDirective`

- `CommandDirective.BuiltIn` -> `GlobalFlag.Action`: Use the global action definition type; it is processed directly by the runner.

- `CommandDirective.CommandDirective` -> `none`: The intermediate parse-result model was removed; the runner invokes the selected handler directly.

- `CommandDirective.UserDefined` -> `none`: The user-defined intermediate directive was removed.

- `CommandDirective.builtIn` -> `GlobalFlag.Action`: Define a custom action flag; v4 runners no longer return built-in directives.

- `CommandDirective.isBuiltIn` -> `none`: Intermediate built-in directives were removed.

- `CommandDirective.map` -> `none`: Map parameters or transform in the handler; there is no intermediate directive to map.

- `CommandDirective.userDefined` -> `none`: Parsed input is delivered directly to the selected command handler.

## `@effect/cli/ConfigFile`

- `ConfigFile.ConfigErrorTypeId` -> `none`: ConfigProvider.SourceError has no public type-id export.

- `ConfigFile.ConfigFileError` -> `ConfigProvider.SourceError`: Use the general source error when implementing a custom file-backed provider.

- `ConfigFile.layer` -> `ConfigProvider.layerAdd(customProviderEffect)`: Build the provider explicitly and add it as fallback to preserve the v3 composition order.

- `ConfigFile.makeProvider` -> `none`: V4 has no API that discovers, parses, and composes config files; use FileSystem, a format parser, and ConfigProvider.fromUnknown explicitly.

## `@effect/cli/HelpDoc`

- `HelpDoc.DescriptionList` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.Empty` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.Enumeration` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.Header` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.HelpDoc` -> `HelpDoc.HelpDoc`: The name remains, but v4 is a structured command-help record rather than a tagged document AST.

- `HelpDoc.Paragraph` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.Sequence` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.blocks` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.descriptionList` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.empty` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.enumeration` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.getSpan` -> `none`: The Span ADT and document-to-span conversion were removed; v4 help fields are strings.

- `HelpDoc.h1` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.h2` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.h3` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.isDescriptionList` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.isEmpty` -> `none`: The Empty variant was removed when HelpDoc became a structured record; inspect the relevant flags, args, subcommands, and examples arrays when an application-specific emptiness test is needed.

- `HelpDoc.isEnumeration` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.isHeader` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.isParagraph` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.isSequence` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.mapDescriptionList` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.orElse` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.p` -> `none`: The v3 document-node ADT was removed; v4 uses a structured HelpDoc record rendered by CliOutput.

- `HelpDoc.toAnsiDoc` -> `none`: CliOutput owns rendering and exposes formatted text rather than an AnsiDoc.

- `HelpDoc.toAnsiText` -> `CliOutput.defaultFormatter().formatHelpDoc`: Format the structured help record; inside Effect code prefer the CliOutput.Formatter service.

## `@effect/cli/Options`

- `Options.All.OptionsAny` -> `Param.AnyFlag`: Use the shared any-flag-parameter type.

- `Options.All.Return` -> `Command.Command.Config.Infer`: Infer the output of a command config record; standalone flag collections were removed.

- `Options.Options` -> `Flag.Flag`: Options was renamed to Flag in effect/cli.

- `Options.Options.BooleanOptionsConfig` -> `Flag.Boolean + Flag.withAlias + Flag.map`: The config object was removed; aliases and value inversion are combinators, while custom negation names need application logic.

- `Options.Options.PathOptionsConfig` -> `{ readonly mustExist?: boolean }`: Path options are inline; true replaces exists=yes and omission replaces either. exists=no has no exact replacement.

- `Options.Options.Variance` -> `Flag.Flag`: The separate variance artifact was removed; Flag inherits the shared Param variance.

- `Options.OptionsTypeId` -> `Param.isParam(value) && value.kind === Param.flagKind`: The public Options type id was removed; use the Param guard and flag kind discriminator.

- `Options.all` -> `Command.make(name, config)`: Collect flags in the config record passed to Command.make; there is no standalone Flag.all.

- `Options.atLeast` -> `Flag.atLeast`: Use the moved combinator; v4 returns ReadonlyArray rather than NonEmptyArray.

- `Options.atMost` -> `Flag.atMost`: Use the moved combinator.

- `Options.between` -> `Flag.between`: Use the moved combinator; v4 validates bounds when constructing the parameter.

- `Options.boolean` -> `Flag.Boolean + Flag.withDefault`: Use Flag.Boolean(name).pipe(Flag.withDefault(false)) to preserve v3's omitted-flag default; bare Flag.Boolean is now required. --no-name is automatic and aliases are added with Flag.withAlias.

- `Options.choice` -> `Flag.Literals`: Use the moved constructor.

- `Options.choiceWithValue` -> `Flag.ChoiceWithValue`: Use the moved constructor.

- `Options.date` -> `Flag.Date`: Use the moved constructor.

- `Options.directory` -> `Flag.Directory`: Use mustExist=true for exists=yes and omit it for either; exists=no has no exact replacement.

- `Options.file` -> `Flag.File`: Use mustExist=true for exists=yes and omit it for either; exists=no has no exact replacement.

- `Options.fileContent` -> `Flag.File + Flag.mapEffect`: Parse a path and read it with FileSystem.readFile; no binary-content flag constructor remains.

- `Options.fileParse` -> `Flag.FileParse`: Pass the old format as an options field; v4 returns parsed content rather than a path/content tuple.

- `Options.fileSchema` -> `Flag.FileSchema`: Pass the old format as an options field and use a v4 Schema constraint decoder.

- `Options.fileText` -> `Flag.File + Flag.mapEffect`: Flag.FileText returns content only; read after Flag.File when the path/content tuple must be preserved.

- `Options.filterMap` -> `Flag.filterMap`: Use the moved combinator and replace the fixed message with an onNone function.

- `Options.float` -> `Flag.Finite`: Use the moved constructor.

- `Options.getHelp` -> `none`: Per-flag help introspection was removed; Command generates help internally.

- `Options.getIdentifier` -> `none`: Public flag identifier introspection was removed.

- `Options.getUsage` -> `none`: The public Usage tree was removed; Command generates a usage string internally.

- `Options.integer` -> `Flag.Int`: Use the moved constructor.

- `Options.isBool` -> `none`: No public flag-shape predicate remains; boolean-shape inspection is internal.

- `Options.isOptions` -> `Param.isParam(value) && value.kind === Param.flagKind`: Flags now use the shared Param representation and an explicit kind discriminator.

- `Options.keyValueMap` -> `Flag.KeyValuePair`: Renamed and now returns Record\<string, string\> rather than HashMap.

- `Options.map` -> `Flag.map`: Use the moved combinator.

- `Options.mapEffect` -> `Flag.mapEffect`: Use the moved combinator; mapping failures are CliError.

- `Options.mapTryCatch` -> `Flag.mapTryCatch`: Use the moved combinator; onError now returns a string rather than HelpDoc.

- `Options.none` -> `omit the config entry`: V4 Flag.Never is an always-failing sentinel, not v3's empty successful option set.

- `Options.optional` -> `Flag.optional`: Use the moved combinator; it still returns Option.

- `Options.orElse` -> `Flag.orElse(() => fallback)`: The fallback is now lazy; add explicit exclusivity validation if both flags must be rejected.

- `Options.orElseEither` -> `Flag.orElseResult(() => fallback)`: Either became Result and the fallback is lazy; v4 no longer rejects both flags being present.

- `Options.parse` -> `flag.parse({ flags, arguments: [] })`: Parsing is now a Param method over a Record and returns leftover arguments with the value; errors are CliError.

- `Options.processCommandLine` -> `Command.runWith`: Raw argv processing is now whole-command execution; no public standalone flag tokenizer remains.

- `Options.redacted` -> `Flag.Redacted`: Use the moved constructor.

- `Options.repeated` -> `Flag.variadic`: Renamed to variadic; pass optional min and max bounds.

- `Options.secret` -> `Flag.Redacted`: The deprecated Secret constructor was removed; use Redacted-backed input.

- `Options.text` -> `Flag.String`: Renamed from text to String.

- `Options.withAlias` -> `Flag.withAlias`: Use the moved combinator.

- `Options.withDefault` -> `Flag.withDefault`: Use the moved combinator; v4 also accepts an Effect fallback.

- `Options.withDescription` -> `Flag.withDescription`: Use the moved combinator.

- `Options.withFallbackConfig` -> `Flag.withFallbackConfig`: Use the moved combinator; invalid configuration becomes CliError.InvalidValue.

- `Options.withFallbackPrompt` -> `Flag.withFallbackPrompt`: Use the moved combinator; v4 can construct the Prompt lazily in Effect.

- `Options.withPseudoName` -> `Flag.withMetavar`: Renamed to withMetavar.

- `Options.withSchema` -> `Flag.withSchema`: Use the moved combinator with a v4 Schema constraint decoder.

## `@effect/cli/Primitive`

- `Primitive.Primitive.PathExists` -> `mustExist?: boolean`: Use true for yes and omit for either; no cannot be represented exactly because false permits existing paths.

- `Primitive.Primitive.ValueType` -> `P extends Primitive.Primitive<infer A> ? A : never`: The named helper was removed; infer the value with a local conditional type.

- `Primitive.Primitive.Variance` -> `Primitive.Primitive.Variance`: The variance interface remains, but its brand key is internal; prefer Primitive\<A\> in user APIs.

- `Primitive.PrimitiveTypeId` -> `none`: The public Primitive type-id symbol was removed.

- `Primitive.boolean` -> `Primitive.Boolean`: Boolean is now a singleton value; defaults belong on Flag.Boolean or withDefault.

- `Primitive.choice` -> `Primitive.Choice`: Use the moved constructor.

- `Primitive.date` -> `Primitive.Date`: Date is now a singleton Primitive value.

- `Primitive.float` -> `Primitive.Finite`: Finite is now a singleton Primitive value and rejects non-finite numbers.

- `Primitive.getChoices` -> `none`: Choice introspection is internal in v4; retain alternatives in application code when needed.

- `Primitive.getHelp` -> `none`: Primitive-level help generation was removed from the public API.

- `Primitive.integer` -> `Primitive.Int`: Int is now a singleton Primitive value.

- `Primitive.isBool` -> `none`: The boolean Primitive predicate is internal in v4.

- `Primitive.text` -> `Primitive.String`: Renamed from text to String.

- `Primitive.validate` -> `primitive.parse(value)`: Parsing is now the Primitive.parse method over a string; defaults and case normalization moved out of this layer.

## `@effect/cli/Prompt`

- `Prompt.All.PromptAny` -> `Prompt.Any`: The any-prompt alias moved out of the All namespace.

- `Prompt.All.Return` -> `Prompt.All.Return`: The collection result helper remains under Prompt.All.

- `Prompt.Prompt` -> `Prompt.Prompt`: The model moved to effect/cli; quitting now fails with Terminal.QuitError.

- `Prompt.Prompt.FloatOptions` -> `Prompt.NumberOptions`: Use the renamed public options type for Prompt.Number; it still extends the integer options type, now IntOptions.

- `Prompt.Prompt.IntegerOptions` -> `Prompt.IntOptions`: Use the renamed public options type for Prompt.Int; option fields are preserved.

- `Prompt.Prompt.Variance` -> `Prompt.Prompt`: The named variance artifact was removed; use Prompt\<Output\>.

- `Prompt.Prompt.VarianceStruct` -> `Prompt.Prompt`: The named variance structure was removed; use Prompt\<Output\>.

- `Prompt.PromptTypeId` -> `Prompt.isPrompt`: The public type-id symbol was removed; use the runtime guard.

- `Prompt.confirm` -> `Prompt.Confirm`: Use the renamed constructor.

- `Prompt.custom` -> `Prompt.Custom`: Use the renamed constructor; both overloads are preserved.

- `Prompt.date` -> `Prompt.Date`: Use the moved constructor.

- `Prompt.file` -> `Prompt.File`: Use the moved constructor; v4 also supports a default selected path.

- `Prompt.float` -> `Prompt.Number`: Use the moved constructor; v4 also supports a default value.

- `Prompt.hidden` -> `Prompt.Hidden`: Use the renamed constructor.

- `Prompt.integer` -> `Prompt.Int`: Use the moved constructor; v4 also supports a default value.

- `Prompt.list` -> `Prompt.List`: Use the renamed constructor.

- `Prompt.multiSelect` -> `Prompt.MultiSelect`: Use the renamed constructor.

- `Prompt.password` -> `Prompt.Password`: Use the renamed constructor.

- `Prompt.select` -> `Prompt.Select`: Use the renamed constructor.

- `Prompt.text` -> `Prompt.String`: Use the moved constructor.

- `Prompt.toggle` -> `Prompt.Toggle`: Use the renamed constructor.

## `@effect/cli/ValidationError`

- `ValidationError.CommandMismatch` -> `none`: The root-command mismatch error was removed.

- `ValidationError.CorrectedFlag` -> `CliError.UnrecognizedOption`: Use the unrecognized-option class and its suggestions field.

- `ValidationError.HelpRequested` -> `CliError.ShowHelp`: Renamed and redesigned as ShowHelp.

- `ValidationError.InvalidArgument` -> `CliError.InvalidValue | CliError.UnexpectedArgument`: Argument decoding and leftover operands are separate v4 errors.

- `ValidationError.InvalidValue` -> `CliError.InvalidValue`: Use the schema-backed v4 error class.

- `ValidationError.MissingFlag` -> `CliError.MissingOption`: Renamed to MissingOption.

- `ValidationError.MissingSubcommand` -> `none`: The dedicated missing-subcommand error was removed.

- `ValidationError.MissingValue` -> `CliError.InvalidValue`: The dedicated tag was folded into InvalidValue.

- `ValidationError.MultipleValuesDetected` -> `CliError.InvalidValue`: Count violations are summarized as InvalidValue without preserving the old values array.

- `ValidationError.NoBuiltInMatch` -> `none`: The intermediate built-in matching error was removed.

- `ValidationError.UnclusteredFlag` -> `none`: The public cluster error was removed.

- `ValidationError.ValidationError` -> `CliError.CliError`: The validation union was redesigned and renamed to CliError.

- `ValidationError.ValidationError.Proto` -> `none`: V4 errors are schema-backed classes and expose no shared public prototype type.

- `ValidationError.ValidationErrorTypeId` -> `none`: The CliError type id is private; use CliError.isCliError.

- `ValidationError.commandMismatch` -> `none`: V4 runners receive arguments after the root name; unknown child commands use CliError.UnknownSubcommand.

- `ValidationError.correctedFlag` -> `new CliError.UnrecognizedOption({ option, command, suggestions })`: Suggestions are carried by UnrecognizedOption; there is no separate corrected-flag case.

- `ValidationError.helpRequested` -> `new CliError.ShowHelp({ commandPath, errors: [] })`: Help requests now carry a command path and optional underlying errors.

- `ValidationError.invalidArgument` -> `new CliError.InvalidValue({ option, value, expected, kind: "argument" })`: Use InvalidValue for undecodable arguments and UnexpectedArgument for leftover operands.

- `ValidationError.invalidValue` -> `new CliError.InvalidValue({ option, value, expected, kind })`: Replace the HelpDoc payload with structured option, value, expected, and kind fields.

- `ValidationError.isCommandMismatch` -> `none`: The root-command mismatch error was removed.

- `ValidationError.isCorrectedFlag` -> `error._tag === "UnrecognizedOption" && error.suggestions.length > 0`: Check the v4 tag and suggestions array.

- `ValidationError.isHelpRequested` -> `error._tag === "ShowHelp"`: Narrow the CliError union by its tag.

- `ValidationError.isInvalidArgument` -> `(error._tag === "InvalidValue" && error.kind === "argument") || error._tag === "UnexpectedArgument"`: Check both v4 argument error forms.

- `ValidationError.isInvalidValue` -> `error._tag === "InvalidValue"`: Narrow the CliError union by its tag.

- `ValidationError.isMissingFlag` -> `error._tag === "MissingOption"`: MissingFlag was renamed to MissingOption.

- `ValidationError.isMissingSubcommand` -> `none`: Missing subcommands now cause ShowHelp rather than a dedicated error.

- `ValidationError.isMissingValue` -> `error._tag === "InvalidValue" && error.value === ""`: Missing values are represented as InvalidValue with an empty value.

- `ValidationError.isMultipleValuesDetected` -> `none`: Count violations are summarized as InvalidValue without a stable subtype.

- `ValidationError.isNoBuiltInMatch` -> `none`: Built-ins are GlobalFlag definitions and the intermediate failure was removed.

- `ValidationError.isUnclusteredFlag` -> `none`: Cluster expansion is internal and has no public intermediate error.

- `ValidationError.isValidationError` -> `CliError.isCliError`: Use the renamed union guard.

- `ValidationError.keyValuesDetected` -> `new CliError.InvalidValue({ option, value, expected, kind: "flag" })`: Represent count violations with structured InvalidValue fields.

- `ValidationError.missingFlag` -> `new CliError.MissingOption({ option })`: MissingFlag was renamed to MissingOption.

- `ValidationError.missingSubcommand` -> `none`: A parent without a selected subcommand now shows help rather than emitting a dedicated error.

- `ValidationError.missingValue` -> `new CliError.InvalidValue({ option, value: "", expected, kind })`: Missing values are represented as InvalidValue with an empty value.

- `ValidationError.noBuiltInMatch` -> `none`: Built-ins are GlobalFlag definitions and the intermediate failure was removed.

- `ValidationError.unclusteredFlag` -> `none`: Flag cluster expansion is internal in v4.
