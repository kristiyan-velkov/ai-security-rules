# AI tools – security prompts (front-end)

These files are for **AI coding assistants** (Cursor, GitHub Copilot, Claude, etc.) so generated **front-end** code follows security rules.

**Supported frameworks:** React.js, Next.js, Remix, TanStack Start, Angular, Analog.js, Vue.js, Nuxt.js.

## Files

| File | Use for |
|------|--------|
| **security-system-prompt.md** | System prompt / custom instructions: paste into your AI tool so it always applies these rules when generating front-end code. |
| **security-checklist.md** | Short checklist for reviews, PRs, or pre-commit: input, XSS, auth, secrets, dependencies, API/HTTPS. |

## Coverage

- **XSS** – No unsanitized HTML; framework-specific (dangerouslySetInnerHTML, v-html, bypassSecurityTrust*, etc.).
- **Auth & sessions** – Token storage (memory/HttpOnly cookies), secure cookies, no open redirects.
- **Sensitive data** – No secrets in client; no logging PII/tokens; no tokens/PII in query params.
- **API & network** – HTTPS, auth headers, CSRF, CORS.
- **Dependencies** – Audits, lockfiles.

## How to use

### Cursor

- **Rules:** Use this repo’s `.cursor/rules/` (synced from `rules/`) or copy `rules/` into your project’s `.cursor/rules/`.
- **Agent:** Use the **frontend-security** subagent (`.cursor/agents/frontend-security.md`) — invoke with `/frontend-security` for security reviews.
- **Checklist:** Add `security-checklist.md` to a rule or your docs; reference it in PR templates or before merging.
- **Package managers:** Copy templates from `package-manager/` (`.npmrc`, `.yarnrc.yml`, `.pnpmrc`) to your project root.

### GitHub Copilot

- Use `security-system-prompt.md` as project-level or custom instructions (e.g. `.github/copilot-instructions.md` or your IDE’s Copilot custom instructions).
- Use `security-checklist.md` in PR templates or as a manual step before accepting suggestions.

### Other AI tools

- **System prompt:** Paste `security-system-prompt.md` into the system prompt / “project rules” / custom instructions.
- **Checklist:** Use `security-checklist.md` when you or the model reviews or generates code.

## One-line summary for prompts

If you only have one line for “security rules” in your AI tool:

> Never put secrets in front-end code or client-exposed env; validate and sanitize all user input; never render unsanitized data as HTML or in javascript: URLs; use HTTPS and secure cookies; send tokens in headers, not query params; never build redirect URLs from user input; run npm audit and fix high/critical.

For full coverage, use **security-system-prompt.md** and the **.cursor/rules/** in this repo (shared + framework-specific for React.js, Next.js, Remix, TanStack Start, Angular, Analog.js, Vue.js, Nuxt.js).
