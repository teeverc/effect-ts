# Versioning, Stability, and Requirements (Effect 4.0.x)

This skill targets **Effect 4.0.1** (stable, LTS). Upstream:
https://github.com/Effect-TS/effect (v3 lives on the `v3` branch).

## One version for the whole ecosystem

- `effect` and every `@effect/*` package are released together with the same
  version number. Pin them identically:

```json
{
  "dependencies": {
    "effect": "4.0.1",
    "@effect/platform-node": "4.0.1",
    "@effect/sql-pg": "4.0.1"
  },
  "devDependencies": {
    "@effect/vitest": "4.0.1"
  }
}
```

- Mismatched versions (e.g. `effect@4.0.1` with `@effect/platform-node@4.0.0-rc.*`)
  can install two copies of `effect`, breaking service identity and type
  compatibility. Upgrade all Effect packages in one step.
- Signs of v3-era code: `@effect/platform@0.x`, `@effect/sql@0.x`,
  `@effect/schema`, `effect@3.x`. Signs of beta-era v4 code: `4.0.0-beta.*` /
  `4.0.0-rc.*` versions, `effect/unstable/*` imports. See `references/migration.md`
  and `references/changelog-4.0.md`.

## Stability tags and semver policy

Every public API is in one of three tiers, marked in JSDoc:

| Tag | Meaning |
|---|---|
| (none) | Stable. Strict semver: breaking changes only in a new major. |
| `@stability unstable` | Breaking changes allowed in **minor** releases (4.x → 4.y). |
| `@stability experimental` | Breaking changes allowed in **patch** releases. |

- Modules under `effect/ai`, `cli`, `cluster`, `devtools`, `eventlog`, `http`,
  `http-api`, `observability`, `persistence`, `process`, `reactivity`, `rpc`,
  `schema`, `socket`, `sql`, `workflow`, `workers` (plus `effect/Arbitrary`,
  `effect/testing/TestSchema`, and parts of `effect/net`, `effect/encoding`) are
  `@stability unstable`. Moving them out of `effect/unstable/*` did not
  stabilize them.
- APIs exposing third-party dependencies are `@stability unstable` too:
  `NodeRedis` / `BunRedis` / `DenoRedis`, `@effect/platform-node/Undici`, `ws`
  options, SQL driver client options, generated provider schemas in
  `@effect/ai-*`, all of `@effect/opentelemetry`, and the `vitest` re-export in
  `@effect/vitest`.
- Check a symbol's tier: `grep -n "@stability" node_modules/effect/dist/<path>.d.ts`
  or read the JSDoc in your editor.
- Practical rule: pin an exact version (or `~4.0.x`) when using unstable areas,
  and read the changelog before bumping minors.

LTS (from the upstream README): at least three years of support; bug fixes for
one year and security fixes for two years after the next major is released.

## Requirements

| Component | Requirement |
|---|---|
| TypeScript | >= 5.9, `strict: true`. TypeScript 7 recommended. |
| Node.js | >= 18 general minimum; `@effect/sql-sqlite-node` needs >= 22.16 (`node:sqlite`). |
| `@effect/vitest` | Vitest `>=5.0.0 <6.0.0`; Node `^22.12.0 \|\| ^24.0.0 \|\| >=26.0.0`. |
| `@effect/doctest` | Vitest 5. |
| `@effect/atom-react` | React `>=19.0.0 <20.0.0`. |
| `@effect/platform-deno` | Deno >= 2.8.3. |
| `@effect/platform-node` `NodeRedis` | peer `redis` (node-redis) `>=5 <7` (not `ioredis`). |
| `effect` | No runtime dependencies. |

## Checking the installed version

```sh
npm ls effect @effect/platform-node      # or: pnpm why effect / yarn why effect
node -p "require('effect/package.json').version"
grep -n '"version"' node_modules/effect/package.json
```

- More than one `effect` version in `npm ls effect` output = duplicate copies;
  dedupe (align all `@effect/*` versions, then reinstall).
- There is no runtime version API in 4.0.x (`ModuleVersion` from v3 is gone).

## Upgrading within 4.x

1. Read `packages/effect/CHANGELOG.md` (and the changelogs of the
   `@effect/*` packages you use) for the target range. Patch notes for
   unstable modules may include breaking changes.
2. Bump `effect` and all `@effect/*` together.
3. `tsc --noEmit`, run tests, then grep for renamed APIs.

## Pitfalls

- Copying v3 or beta snippets (`Context.Tag`, `Effect.Service`,
  `effect/unstable/http`, `Schema.makeUnsafe`, `ServiceMap`) into 4.0.x code.
- Assuming an `effect/http` API is semver-stable because it is no longer under
  `unstable/`.
- Upgrading `effect` without upgrading `@effect/vitest` / platform packages.

## Docs

- Changelog digest for this skill: `references/changelog-4.0.md`
- API reference: https://effect.website/docs/v4/api/effect (per package:
  `/docs/v4/api/<package>`, e.g. `platform-node`, `sql-pg`, `vitest`)
- Release notes: https://github.com/Effect-TS/effect/blob/main/packages/effect/CHANGELOG.md
- Migration guide: https://github.com/Effect-TS/effect/blob/main/MIGRATION.md
