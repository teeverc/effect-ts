# effect-ts skills

Effect-TS (Effect) guidance for TypeScript with support for both **Effect v3** and **Effect v4 (stable, 4.0.x)**. Choose the skill that matches your project:

## Skills

### `effect-ts` - Effect v3

Effect v3 (3.19.19) production guidance: designing and implementing Effect-based code, modeling expected errors vs defects, managing dependencies with Context/Layer/Effect.Service, handling resource lifecycles with Scope, running effects at the program edge, using Effect.gen, validating data with Effect Schema, and testing time with TestClock.

**Install:**
```bash
npx skills add teeverc/effect-ts --skill effect-ts
```

**Location:** `./skills/effect-ts/`

**Includes:**
- `SKILL.md` - Core guidance
- `references/` - Comprehensive v3 reference guides

### `effect-v4` - Effect v4 (Stable) + Migration

Effect v4 (stable, `effect@4.0.x` from [Effect-TS/effect](https://github.com/Effect-TS/effect)) development guidance and v3 → v4 migration support. Includes the official agent guide (`LLMS.md`), the official typechecked examples, the official migration guides, and the generated v3 → v4 API rename map.

**Install:**
```bash
npx skills add teeverc/effect-ts --skill effect-v4
```

**Location:** `./skills/effect-v4/`

**Includes:**
- `SKILL.md` - v4 ground rules (imports, versions, stability), core style, triage map
- `references/llms.md` + `references/examples/` - official Effect agent guide and examples (verbatim from upstream)
- `references/*.md` - topic guides (core, errors, services/layers, schema, streams, HTTP, SQL, CLI, AI, RPC/cluster, observability, testing)
- `references/changelog-4.0.md` - what changed from beta to stable 4.0.x
- `references/migration.md` + `references/migration/` - official v3 → v4 migration guides
- `references/migration/api-map/` - per-module v3 → v4 API rename map

### Refreshing from upstream

The verbatim files can be refreshed from a clone of `Effect-TS/effect`:

```bash
UP=/path/to/effect
cp -r $UP/ai-docs/src/. skills/effect-v4/references/examples/
sed 's#](\./ai-docs/src/#](examples/#g' $UP/LLMS.md > skills/effect-v4/references/llms.md
for f in cause equality error-handling fiber-keep-alive fiberref forking generators \
  layer-memoization runtime schema scope services yieldable; do
  cp $UP/migration/$f.md skills/effect-v4/references/migration/$f.md
done
```

`references/migration/fiber-keep-alive.md` carries a local 4.0.1 correction note at the top; re-add it after refreshing.

`references/migration/api-map/` is split from `$UP/migration/v3-to-v4.md` (one file per v3 package, one per `effect/*` module).

## Packaging

Package v3 skill:
```bash
cd skills/effect-ts
zip -r ../../dist/effect-ts.skill SKILL.md references
cd ../..
```

Package v4 skill:
```bash
cd skills/effect-v4
zip -r ../../dist/effect-v4.skill SKILL.md references
cd ../..
```

Both skills can be installed and used independently. Use `effect-v4` for Effect 4.x projects and v3 → v4 migrations; use `effect-ts` for projects still on Effect 3.x.
