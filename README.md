# effect-ts skill

Effect-TS (Effect) guidance for TypeScript targeting Effect v4 (beta): designing and implementing Effect-based code, modeling expected errors vs defects, managing dependencies with ServiceMap/Layer/ServiceMap.Service, handling resource lifecycles with Scope, running effects at the program edge, using Effect.gen, validating data with Schema (Codec), and testing time with TestClock.

Note: v4 introduces `effect/unstable/*` modules that can change in minor releases. These docs focus on stable APIs unless explicitly marked otherwise.

## Install

```bash
npx skills add teeverc/effect-ts-skills --skill effect-ts
```

## What’s included

- `SKILL.md`
- `references/` (focused Effect patterns and guidance)
- `references/migration-v4.md` (v3 → v4 migration checklist)

## Package

```bash
zip -r dist/effect-ts.skill SKILL.md references
```
