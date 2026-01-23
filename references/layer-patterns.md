# Layer Patterns

Use this guide when wiring services and environments.

- Use layers to build dependency graphs; keep construction and acquisition in layers.
- Combine layers with `Layer.merge` or `Layer.provideMerge` to assemble environments.
- Convert a layer to an effect with `Layer.launch` when you need acquisition as an effect.
- Use `Layer.effect` for simple effectful construction and `Layer.scoped` for resources with lifetimes.
- Layers are memoized by reference equality; reuse layer instances to avoid duplicate acquisition.
- In tests, build stub layers and provide them to the effect under test.
