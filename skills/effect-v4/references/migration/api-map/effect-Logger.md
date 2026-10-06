# v3 → v4 API map: effect/Logger

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Logger`

- `Logger.Logger` -> `Logger.Logger`: The name remains. Logger.Options now has fiber instead of fiberId; read the id from fiber.id and annotations or spans through fiber references.

- `Logger.Logger.Variance` -> `none`: Public variance metadata was removed; use Logger.Logger\<Message, Output\> directly.

- `Logger.LoggerTypeId` -> `Logger.isLogger`: The brand is private in v4; use the public runtime guard.

- `Logger.add` -> `Logger.layer([logger], { mergeWithExisting: true })`: Logger installation is whole-set based; mergeWithExisting reproduces add.

- `Logger.addEffect` -> `Logger.layer([loggerEffect], { mergeWithExisting: true })`: Logger.layer accepts effects that construct loggers.

- `Logger.addScoped` -> `Logger.layer([scopedLoggerEffect], { mergeWithExisting: true })`: Layer acquisition handles the scoped effect; the separate scoped constructor is gone.

- `Logger.batched` -> `Logger.batched(logger, { window, flush })`: The trailing arguments moved into one options object. Provide any services needed by flush before constructing it.

- `Logger.filterLogLevel` -> `Logger.make(options => predicate(options.logLevel) ? Option.some(logger.log(options)) : Option.none())`: No named combinator remains; rebuild the wrapper with Logger.make. Prefer References.MinimumLogLevel for ordinary threshold filtering.

- `Logger.json` -> `Logger.layer([Logger.consoleJson, Logger.tracerLogger])`: Logger.layer replaces the active set. Include tracerLogger to preserve v3 built-in layer behavior, or omit it when trace log events are intentionally disabled.

- `Logger.jsonLogger` -> `Logger.formatJson`: Formatter rename; v4 JSON output uses level rather than logLevel.

- `Logger.logFmt` -> `Logger.layer([Logger.consoleLogFmt, Logger.tracerLogger])`: Logger.layer replaces the active set; include tracerLogger to preserve v3 built-in layer behavior.

- `Logger.logfmtLogger` -> `Logger.formatLogFmt`: Formatter rename and capitalization change.

- `Logger.map` -> `Logger.map`: Retained with the same output-mapping behavior.

- `Logger.mapInput` -> `Logger.make(options => logger.log({ ...options, message: f(options.message) }))`: No named input contramap remains; rebuild it with Logger.make.

- `Logger.mapInputOptions` -> `Logger.make(options => logger.log(f(options)))`: No named options contramap remains; rebuild it with Logger.make and adapt f to the v4 Logger.Options shape.

- `Logger.minimumLogLevel` -> `Layer.succeed(References.MinimumLogLevel, level)`: Minimum log level is now a context reference.

- `Logger.none` -> `Logger.make(() => undefined)`: Rebuild the no-op logger with Logger.make.

- `Logger.pretty` -> `Logger.layer([Logger.consolePretty(), Logger.tracerLogger])`: Logger.layer replaces the active set; include tracerLogger to preserve v3 built-in layer behavior.

- `Logger.prettyLogger` -> `Logger.consolePretty`: Renamed to consolePretty. Remove the stderr option; provide Logger.LogToStderr with true to route TTY output to console.error. Colors, formatDate, and mode remain constructor options.

- `Logger.prettyLoggerDefault` -> `Logger.consolePretty()`: The prebuilt singleton became a constructor call.

- `Logger.remove` -> `Logger.layer([...desiredLoggers])`: No named removal combinator remains. Declare the complete desired logger set; transform Logger.CurrentLoggers only when removing from an unknown inherited set is unavoidable.

- `Logger.replace` -> `Logger.layer([...desiredLoggers])`: V4 replaces the whole active set. When replacing the old default logger, include Logger.tracerLogger explicitly if it must survive.

- `Logger.replaceEffect` -> `Logger.layer([loggerEffect, ...otherLoggers])`: Logger.layer accepts effects. Explicitly list every logger that must remain active.

- `Logger.replaceScoped` -> `Logger.layer([scopedLoggerEffect, ...otherLoggers])`: Logger.layer acquisition supplies the scope; explicitly list every logger that must remain active.

- `Logger.simple` -> `Logger.make(({ message }) => log(message))`: V3 simple was a message-only custom logger constructor; rebuild it with Logger.make.

- `Logger.stringLogger` -> `Logger.formatSimple`: The prebuilt string formatter was renamed.

- `Logger.structured` -> `Logger.layer([Logger.consoleStructured, Logger.tracerLogger])`: Logger.layer replaces the active set; include tracerLogger to preserve v3 built-in layer behavior.

- `Logger.structuredLogger` -> `Logger.formatStructured`: Formatter rename; its output field logLevel is now named level.

- `Logger.succeed` -> `Logger.make(() => value)`: Rebuild the constant-output logger with Logger.make.

- `Logger.sync` -> `Logger.make(() => evaluate())`: Rebuild the lazy-output logger; evaluate still runs once per log event.

- `Logger.test` -> `Effect.log(input).pipe(Effect.provide(Logger.layer([capturingLogger])))`: No synthetic-options helper remains. Exercise the logger through the runtime and capture its output so it receives a real Fiber, cause, level, and date.

- `Logger.withMinimumLogLevel` -> `Effect.provideService(effect, References.MinimumLogLevel, level)`: Replace the FiberRef-local helper with reference provisioning.

- `Logger.withSpanAnnotations` -> `custom Logger.make wrapper using options.fiber.cache.span`: No transparent generic equivalent remains. Read span identity from options.fiber.cache.span and add it to custom output as needed.

- `Logger.zip` -> `Logger.make(options => [left.log(options), right.log(options)])`: No named combinator remains; invoke both loggers and return their output tuple.

- `Logger.zipLeft` -> `Logger.make(options => { const output = left.log(options); right.log(options); return output })`: Rebuild explicitly, preserving evaluation of both loggers and returning the left output.

- `Logger.zipRight` -> `Logger.make(options => { left.log(options); return right.log(options) })`: Rebuild explicitly, preserving evaluation order and returning the right output.
