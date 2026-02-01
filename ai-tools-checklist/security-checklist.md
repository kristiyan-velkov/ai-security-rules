# Front-end security checklist

Use this when **reviewing** or **generating** front-end code. Tick each item before merging or shipping.

Supported stacks: React.js, Next.js, Remix, TanStack Start, Angular, Analog.js, Vue.js, Nuxt.js.

## General

- [ ] All user input is validated and sanitized before use (display, URLs, API payloads).
- [ ] No secrets (API keys, tokens, passwords) in source code or committed env files or client-exposed config (`NEXT_PUBLIC_*`, `VITE_*`, `runtimeConfig.public`).
- [ ] HTTPS only in production; secure cookies (Secure, HttpOnly, SameSite) where applicable.

## Input and output

- [ ] No unsanitized user or external data rendered as HTML (`innerHTML`, `dangerouslySetInnerHTML`, `v-html`, `{@html}`, `bypassSecurityTrust*` without sanitizer).
- [ ] No `eval()`, `new Function()`, or dynamic script execution with user-controlled input.
- [ ] URLs validated before use in links or redirects; reject `javascript:` and unsafe `data:`.
- [ ] Redirect URLs are not built from user input (no open redirect).

## Secrets and auth

- [ ] Tokens sent in `Authorization` header (or secure cookie), not in query params.
- [ ] Cookies use `Secure`, `SameSite` (Strict or Lax), and `HttpOnly` where appropriate for auth.
- [ ] OAuth/OIDC redirect URIs allowlisted; redirect URL not built from user input.
- [ ] Sensitive session data cleared on logout; no logging of tokens or PII.

## Dependencies and network

- [ ] Lockfiles committed; CI uses frozen installs (`npm ci` or equivalent).
- [ ] `npm audit` (or equivalent) run; high/critical issues fixed or explicitly accepted.
- [ ] All API calls over HTTPS; CSRF used for state-changing requests when cookie-based auth; CORS not disabled for convenience.

## Quick “never do” list

- Never put secrets in front-end code or in client-exposed env.
- Never render unsanitized user data as HTML or in `javascript:` URLs.
- Never store auth tokens in `localStorage` when HttpOnly cookies or memory are feasible.
- Never send tokens or PII in query params.
- Never disable HTTPS, CORS, or certificate checks in production.
- Never build redirect URLs from user input (open redirect).
- Never log or send passwords, tokens, or unredacted PII to analytics or error reporting.

---

**Usage:** Paste this into a Cursor rule, a PR template, or a pre-commit checklist. For full AI code-generation guidance, use `security-system-prompt.md`.
