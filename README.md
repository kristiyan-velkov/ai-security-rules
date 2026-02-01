# AI Security Rules (Front-End)

<p align="center">
  <img src="images/ai-front-end-security-rules-logo.png" alt="AI Front-End Security Rules" width="400" />
</p>

AI Security rules and best practices for **front-end** applications, designed to be committed to your repo and used by AI coding tools (**Cursor**, **GitHub Copilot**, **Claude**, **Kiro**, and others) so generated code stays secure by default.

## What’s in this repo

- **General security rule** – Always-on principles (never trust input, no secrets in code, secure defaults).
- **Shared front-end rules** – XSS, auth/sessions, sensitive data, dependencies, API/network, CSP & clickjacking, open redirect (apply to all apps).
- **Framework-specific rules** – One rule file per supported framework (see table below).
- **AI tool prompts** – Ready-to-use prompts and checklists for AI code generation.

## Supported frameworks

| Framework       | Version        | Directory        | Port | Rule file          |
|----------------|----------------|------------------|------|--------------------|
| React.js       | v19.2.3        | `react.js`       | 8080 | `frontend-specific/react.mdc` |
| Next.js        | v16.1.1        | `next.js`        | 3000 | `frontend-specific/nextjs.mdc` |
| Remix.js       | React Router v7.10 | `remix.js/`   | 3000 | `frontend-specific/remix.mdc` |
| TanStack Start | v1.132         | `tanstack-start/` | 3000 | `frontend-specific/tanstack-start.mdc` |
| Angular        | v21            | `angular/`       | 8080 | `frontend-specific/angular.mdc` |
| Analog.js      | v2.2 (Angular 21) | `analog.js`   | 3000 | `frontend-specific/analog.mdc` |
| Vue.js         | v3.5           | `vue.js`         | 8080 | `frontend-specific/vue.mdc` |
| Nuxt.js        | v4.2           | `nuxt.js`        | 3000 | `frontend-specific/nuxt.mdc` |

Rules are applied by **directory** and **file patterns**: framework rules use broad globs (e.g. `**/app/**/*.tsx`, `**/next.config.*`) so they apply to typical layouts (`src/`, `app/`, etc.) even if you don't use a folder named after the framework. Adjust the `globs` in each `.mdc` file if needed.

## Quick start

### Option 1: Use as a submodule (recommended)

```bash
git submodule add https://github.com/YOUR_ORG/ai-security-rules.git .security-rules
cp -r .security-rules/rules ./
```

Then commit `rules/` so everyone (and AI) uses the same rules. **Cursor users:** copy `rules/` into your project as `.cursor/rules/` so Cursor picks them up automatically.

### Option 2: Copy rules into your project

1. Clone or download this repo.
2. Copy the contents of `rules/` into your project (e.g. `rules/` at repo root, or `.cursor/rules/` if you use Cursor).
3. Commit the rules.

### Option 3: Reference in AI tools

- Use the prompts in `ai-tools-checklist/` as system prompts, custom instructions, or “rules” in your AI coding tool.
- Paste the checklist into a rule file or docs so the model (or you) can verify each change.

## Rule structure

Rules live in **`rules/`** at the repo root. **Cursor users:** copy `rules/` to `.cursor/rules/` in your project so Cursor picks them up automatically. For **Claude**, **Kiro**, **GitHub Copilot**, and other AI tools, use the prompts in `ai-tools-checklist/` as system prompts or custom instructions, or copy the rule content into your tool’s rules/preferences.

```
rules/
├── general/           # Always apply
│   └── general-security.mdc
└── frontend-specific/ # Shared + framework-specific (Cursor, Claude, Kiro, etc.)
    ├── xss.mdc
    ├── auth-sessions.mdc
    ├── sensitive-data.mdc
    ├── dependencies.mdc
    ├── api-network.mdc
    ├── csp-headers.mdc
    ├── open-redirect.mdc
    ├── react.mdc
    ├── nextjs.mdc
    ├── remix.mdc
    ├── tanstack-start.mdc
    ├── angular.mdc
    ├── analog.mdc
    ├── vue.mdc
    └── nuxt.mdc
```

## Rule reference

### General (always apply)

| File | Purpose |
|------|--------|
| `general/general-security.mdc` | Core principles: never trust input, no secrets in code, least privilege, secure defaults, defense in depth. |

### Shared front-end rules

| File | Purpose |
|------|--------|
| `frontend-specific/xss.mdc` | XSS prevention: no unsanitized HTML, safe URLs, CSP-friendly patterns. |
| `frontend-specific/auth-sessions.mdc` | Token storage, cookies (HttpOnly, SameSite, Secure), auth flows. |
| `frontend-specific/sensitive-data.mdc` | PII, logging, storage, no secrets in client. |
| `frontend-specific/dependencies.mdc` | Supply chain: audits, lockfiles, trusted deps, SRI for third-party scripts. |
| `frontend-specific/api-network.mdc` | HTTPS, auth headers, CSRF, CORS, input validation. |
| `frontend-specific/csp-headers.mdc` | Content-Security-Policy, framing (clickjacking). |
| `frontend-specific/open-redirect.mdc` | Open redirect prevention: allowlist redirect targets. |

### Framework-specific rules

| File | App directory | Purpose |
|------|----------------|--------|
| `frontend-specific/react.mdc` | `react.js/` or `**/*.tsx` | React.js: dangerouslySetInnerHTML, state, routing, env. |
| `frontend-specific/nextjs.mdc` | `next.js/` or `**/next.config.*`, `app/` | Next.js: NEXT_PUBLIC_*, Server Components, middleware, Server Actions. |
| `frontend-specific/remix.mdc` | `remix.js/` or `app/`, `remix.config.*` | Remix: loaders/actions, cookies, redirects. |
| `frontend-specific/tanstack-start.mdc` | `tanstack-start/` or `app/` | TanStack Start: server functions, cookies, redirects. |
| `frontend-specific/angular.mdc` | `angular/` or `**/*.component.*` | Angular: DomSanitizer, bypassSecurityTrust*, HttpClient, guards. |
| `frontend-specific/analog.mdc` | `analog.js/` or `app/` | Analog.js: Angular + server routes, env, cookies. |
| `frontend-specific/vue.mdc` | `vue.js/` or `**/*.vue` | Vue.js: v-html, state, Vue Router, provide/inject. |
| `frontend-specific/nuxt.mdc` | `nuxt.js/` or `**/nuxt.config.*`, `**/*.vue` | Nuxt.js: runtimeConfig.public, server routes, useCookie. |

Rules in **`rules/`** can be copied to `.cursor/rules/` for **Cursor** (picked up automatically when the right files are open). For **Claude**, **Kiro**, and other tools, use `ai-tools-checklist/security-system-prompt.md` as a system prompt or paste rule content into your tool’s custom instructions.

## Security coverage

- **XSS** – No unsanitized HTML; framework-specific (dangerouslySetInnerHTML, v-html, bypassSecurityTrust*, etc.).
- **Auth & sessions** – Token storage (prefer memory/HttpOnly cookies), secure cookies (Secure, SameSite), auth flows; see open-redirect for redirect URIs.
- **Sensitive data** – No secrets in client code; no logging PII/tokens; minimize storage; no tokens/PII in query params; no secrets/PII in SSR payloads.
- **CSRF & CORS** – CSRF tokens for state-changing requests when using cookie-based auth; explicit CORS; HTTPS only.
- **CSP & clickjacking** – Strict CSP where possible; framing protection (X-Frame-Options / frame-ancestors).
- **Open redirect** – Allowlist redirect targets; never use user input as redirect URL.
- **Dependencies** – Audits, lockfiles, trusted packages, SRI for third-party scripts.

## Using the AI tool prompts

The `ai-tools-checklist/` folder contains prompts you can plug into any AI coding assistant:

- **`ai-tools-checklist/security-system-prompt.md`** – Full system-style prompt for secure front-end code. Use as custom instruction, rule, or prepended context.
- **`ai-tools-checklist/security-checklist.md`** – Short checklist for pre-commit or code review (input, secrets, XSS, auth, dependencies, API/HTTPS).

Use them in **Cursor** (Rules), **GitHub Copilot** (project instructions), **Claude** (project or custom instructions), **Kiro**, or any other AI coding tool (system prompt / project rules).

## Project structure

```
ai-security-rules/
├── rules/
│   ├── general/
│   │   └── general-security.mdc
│   └── frontend-specific/
│       ├── xss.mdc, auth-sessions.mdc, sensitive-data.mdc
│       ├── dependencies.mdc, api-network.mdc
│       ├── csp-headers.mdc, open-redirect.mdc
│       ├── react.mdc, nextjs.mdc, remix.mdc, tanstack-start.mdc
│       ├── angular.mdc, analog.mdc, vue.mdc, nuxt.mdc
├── ai-tools-checklist/
│   ├── README.md
│   ├── security-system-prompt.md
│   └── security-checklist.md
├── images/
│   └── ai-front-end-security-rules-logo.png
└── README.md
```

## Rule quality and scope

These rules aim for **production-ready** front-end security: they cover XSS, auth, sensitive data, API/network, dependencies, CSP, open redirect, and framework-specific pitfalls. Rules are short and actionable (red flags, do/don’t). Shared rules cross-link (e.g. auth-sessions ↔ api-network, xss ↔ csp-headers). Framework globs are broadened so rules apply to typical layouts (`src/`, `app/`, `**/*.tsx`, config files) even without a framework-named folder.

## Customizing

- **Directories:** If your app lives in a different folder (e.g. `apps/next`), edit the `globs` frontmatter in the framework `.mdc` file (e.g. add `**/apps/next/**`).
- **Strictness:** Relax or tighten rules (e.g. documented exception for localStorage tokens).
- **New frameworks:** Add a new `.mdc` file under `rules/frontend-specific/` with the right `globs` for that app.

## Contributing

Improvements and extra rules are welcome. Please:

- Keep rules short and actionable (aim for &lt; 50 lines per rule when possible).
- Use concrete “do / don’t” examples.
- Open an issue or PR with the change and a short rationale.


## ☕ Support My Work

If you find this useful, consider giving the repo a **⭐️ star** — it helps others discover it.

If you'd like to support me further, you can donate via:

- [Revolut](https://revolut.me/kristiyanvelkov)
- [Buy Me a Coffee](https://www.buymeacoffee.com/kristiyanvelkov)
- [GitHub Sponsors](https://github.com/sponsors/kristiyan-velkov)

Your support helps me continue creating valuable content for the community. Thank you!

---

## 📬 Contact me

If you'd like to connect, feel free to reach out via:

- [LinkedIn](https://www.linkedin.com/in/kristiyan-velkov-763130b3/)
- [X.com](https://x.com/krisvelkov)



## License

Use and adapt these rules in your projects. If you redistribute this repo, keep the same license and attribution as in the repository.
