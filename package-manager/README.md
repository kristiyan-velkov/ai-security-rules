# Package manager security configs

Hardened defaults for **npm**, **Yarn**, and **pnpm** to reduce supply-chain risk. Copy the file for your package manager into your **project root** (package managers only read config from the repo root).

| File | Package manager | Copy to project root as |
|------|-----------------|-------------------------|
| `.npmrc` | npm | `.npmrc` |
| `.yarnrc.yml` | Yarn Berry (v2+) | `.yarnrc.yml` |
| `.pnpmrc` | pnpm | `.pnpmrc` |

## What these settings do

- **Block install scripts by default** (`ignore-scripts` / `enableScripts: false`) — mitigates malicious postinstall scripts.
- **Exact versions** — no floating `^` / `~` ranges when adding deps (`save-exact`, `defaultSemverRangePrefix: ""`, `savePrefix=""`).
- **Audit & trust** — npm: `audit-level=high`, attestations; pnpm: `trustPolicy=no-downgrade`, store integrity checks.
- **Minimum release age** — delay installing brand-new publishes (npm: 7 days; pnpm: 10080 minutes) to reduce typosquat / rushed malicious releases.
- **Strict engines & peers** — fail installs on Node engine mismatch or peer dependency problems.
- **Reproducible installs** — lockfiles, immutable installs (Yarn), isolated `node_modules` (pnpm).
- **Dependency confusion** — comments show how to map `@your-org/*` scopes to a private registry (required for internal packages).

## Quick start

```bash
# npm
cp package-manager/.npmrc ./

# Yarn Berry
cp package-manager/.yarnrc.yml ./

# pnpm
cp package-manager/.pnpmrc ./
```

Then run your usual install (`npm ci`, `yarn install --immutable`, `pnpm install`).

## Allowing specific install scripts

With scripts disabled globally, packages like `esbuild` or `sharp` may need their build step. **Review the package**, then allow it explicitly:

- **pnpm:** set `allowBuilds: { package-name: true }` in `.pnpmrc`
- **npm:** use `ignore-scripts=false` only for vetted packages, or run `npm rebuild <pkg>` after audit
- **Yarn:** set `enableScripts: true` temporarily or use `yarn npm` policies per package

## Monorepos (pnpm)

This folder provides **`.pnpmrc`** (tool settings). For a pnpm workspace you still need a separate **`pnpm-workspace.yaml`** at the repo root listing packages, for example:

```yaml
packages:
  - 'apps/*'
  - 'packages/*'
```

## Customization

- Point `registry` / `npmRegistryServer` at your private registry.
- Uncomment and set scope mappings (`@your-org:registry=...`) for dependency-confusion protection.
- Relax `min-release-age` / `minimumReleaseAge` if you need bleeding-edge patches (accept higher risk).

See also: `rules/frontend-specific/dependencies.mdc` for dependency and supply-chain rules used by AI assistants.
