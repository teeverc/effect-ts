# Micro (Bundle-Size Focused Effect Subset)

Use this guide when targeting constrained runtimes where bundle size is a major requirement and full `effect` is not necessary.

## Mental model

- `Micro` is a lightweight alternative for selected use cases.
- Choose `Micro` intentionally per package/runtime boundary, not ad hoc per function.
- Keep interfaces stable so implementations can evolve from `Micro` to full `Effect` if needed.

## Decision guidance

- Prefer full `effect` when you need advanced concurrency, rich ecosystem integrations, or broad module support.
- Consider `Micro` in frontend/edge code paths where bundle size dominates and requirements are narrow.
- Avoid mixing `Micro` and full `Effect` deeply inside the same domain module unless there is a clear boundary.

## Migration workflow

1. Identify boundary modules where lightweight runtime matters most.
2. Keep domain interfaces pure and runtime-agnostic.
3. Swap implementation modules incrementally and verify behavior parity.
4. Add tests around cancellation/error semantics before and after migration.

## Pitfalls

- Chasing bundle size before measuring actual impact.
- Mixing abstractions without a clear module boundary.
- Assuming all full Effect APIs are available in the lightweight subset.

## Docs

- `https://effect.website/docs/micro/new-users/`
- `https://effect.website/docs/micro/effect-users/`
