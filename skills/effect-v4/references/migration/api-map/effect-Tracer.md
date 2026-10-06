# v3 → v4 API map: effect/Tracer

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Tracer`

- `Tracer.DisablePropagation` -> `Tracer.DisablePropagation`: Keep the reference value. The separate phantom interface is gone; the Context.Reference directly stores boolean.

- `Tracer.ExternalSpan` -> `Tracer.ExternalSpan`: Keep the type, but rename the context field to annotations. Apply the same rename to Tracer.externalSpan options.

- `Tracer.ParentSpan` -> `Tracer.ParentSpan`: Keep the API. It is now a Context.Service class for AnySpan rather than a separate phantom interface plus Context.Tag.

- `Tracer.Span` -> `Tracer.Span`: Keep the type and rename span.context to span.annotations; the other public fields and methods remain.

- `Tracer.SpanLink` -> `Tracer.SpanLink`: Keep the type but remove the \_tag property; v4 links contain only span and attributes.

- `Tracer.SpanOptions` -> `Tracer.SpanOptions`: Keep the type and rename context to annotations. V4 splits trace options and additionally accepts sampled and level.

- `Tracer.Tracer` -> `Tracer.Tracer`: The service is now a defaulted Context.Reference. Custom implementations are structural and receive one span options object; context is optional and now receives an Effect primitive plus Fiber.

- `Tracer.TracerTypeId` -> `none`: Tracer implementations are structural and no longer carry a public type-id brand.

- `Tracer.tracerWith` -> `Tracer.Tracer.use`: Replace tracerWith(f) with Tracer.Tracer.use(f); do not use TracerKey, which is only the raw string key.
