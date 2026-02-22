# effect-ts skills

Effect-TS (Effect) guidance for TypeScript with support for both **stable v3** and **beta v4**. Choose the skill that matches your project:

## Skills

### `effect-ts` - Effect v3 (Stable Production)

Effect v3 (3.19.19) production guidance: designing and implementing Effect-based code, modeling expected errors vs defects, managing dependencies with Context/Layer/Effect.Service, handling resource lifecycles with Scope, running effects at the program edge, using Effect.gen, validating data with Effect Schema, and testing time with TestClock.

**Install:**
```bash
npx skills add teeverc/effect-ts --skill effect-ts
```

**Includes:**
- `./SKILL.md` - Core guidance
- `./references/` - Comprehensive v3 reference guides
- All v3 patterns and APIs for production use

### `effect-v4` - Effect v4 (Beta) + Migration

Effect v4 (beta) development guidance and v3 → v4 migration support. The core programming model is stable, but `effect/unstable/*` modules may receive breaking changes in minor releases.

**Install:**
```bash
npx skills add teeverc/effect-ts --skill effect-v4
```

**Includes:**
- `./skills/effect-v4/SKILL.md` - v4 core patterns + migration guidance
- `./skills/effect-v4/references/core-usage.md` - v4 types and combinators
- `./skills/effect-v4/references/dependency-management.md` - ServiceMap patterns
- `./skills/effect-v4/references/generators.md` - Yieldable handling
- `./skills/effect-v4/references/schema.md` - Codec patterns
- `./skills/effect-v4/references/testing-stack.md` - v4 test wiring
- `./skills/effect-v4/references/migration/` - 12 v3 → v4 migration guides

## Packaging

Package v3 skill:
```bash
zip -r dist/effect-ts.skill SKILL.md references
```

Package v4 skill:
```bash
cd skills/effect-v4
zip -r ../../dist/effect-v4.skill SKILL.md references
cd ../..
```

Both skills can be installed and used independently. Recommend v3 for production, v4 for beta testing and migration planning.
