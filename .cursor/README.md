# Cursor setup (AI Security Rules)

This folder configures **Cursor** for this repo: rules (always-on + file-scoped) and a **security subagent**.

## Layout

```
.cursor/
├── README.md                 # This file
├── agents/
│   └── frontend-security.md  # Subagent: /frontend-security or delegated by Agent
└── rules/
    ├── general/
    │   └── general-security.mdc      # alwaysApply: true
    └── frontend-specific/
        ├── xss.mdc, auth-sessions.mdc, sensitive-data.mdc
        ├── dependencies.mdc, api-network.mdc
        ├── csp-headers.mdc, open-redirect.mdc
        └── react.mdc, nextjs.mdc, vue.mdc, nuxt.mdc, …
```

## Rules (`rules/`)

| Path | Applies when |
|------|----------------|
| `rules/general/general-security.mdc` | **Always** — every chat |
| `rules/frontend-specific/*.mdc` | Matching **globs** (e.g. `**/*.tsx`, `**/*.vue`, `package.json`) |

**Canonical source:** edit files under repo root **`rules/`**, then copy into `.cursor/rules/`:

```bash
cp -r rules/general .cursor/rules/
cp -r rules/frontend-specific .cursor/rules/
```

In consumer projects: copy `rules/` → `.cursor/rules/` (or submodule + copy).

## Agent (`agents/frontend-security.md`)

Specialized subagent for front-end security reviews.

**Invoke explicitly:**

```text
/frontend-security review the auth callback changes
```

```text
Use the frontend-security agent on the login form and API client
```

**Automatic delegation:** Agent may delegate when the task involves auth, XSS, secrets, CSP, redirects, or dependency security (see `description` in the agent frontmatter).

The agent is **read-only** (`readonly: true`) — it audits and reports; it does not edit files unless you run Agent mode without readonly.

## Sync with `rules/`

Keep `.cursor/rules/` in sync with `rules/` after changes:

```bash
cp -r rules/general .cursor/rules/
cp -r rules/frontend-specific .cursor/rules/
```

## Related

- **`ai-tools-checklist/`** — system prompt and checklist for Copilot, Claude, Kiro, etc.
- **`package-manager/`** — hardened `.npmrc`, `.yarnrc.yml`, `.pnpmrc` templates
