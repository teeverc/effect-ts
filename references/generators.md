# Generators (Effect.gen)

Use this guide when sequential logic would be clearer than pipelines.

- Use `Effect.gen` to write generator-based code for sequential effects.
- `yield*` within the generator to extract values from effects in order.
- Prefer generators for multi-step workflows, branching, or early returns where pipelines become noisy.
