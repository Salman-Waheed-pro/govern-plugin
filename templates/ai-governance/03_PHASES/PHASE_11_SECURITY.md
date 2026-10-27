# Phase 11 — Security, Supply Chain & AI-Specific Risk (DevSecOps)

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** security is built into the pipeline, not inspected at the end. This phase actively hunts vulnerabilities in the actual code — frontend, backend, and infrastructure — including risks specific to AI-generated code and AI-powered features. **Production launch is forbidden until this phase passes.**

The checks below are the floor, not the ceiling: the agent must also apply any stack-specific vulnerability knowledge beyond this list (see `05_RUNBOOKS/Security.md` for tools).

---

## Objectives — automated guards (permanent, in CI)

- [ ] Dependency vulnerability scanning (Dependabot / `npm audit` / `pip-audit` / `osv-scanner`), alerts on, auto-PRs where available
- [ ] Secrets scanning in CI (gitleaks/trufflehog/GitHub secret scanning) + ONE full git-history scan
- [ ] SAST in CI (CodeQL / Semgrep / Bandit) — findings triaged, not ignored
- [ ] Container image scanning in CI (Trivy/Grype), failing on critical severity

## Objectives — injection, access control, crypto (core OWASP)

**Injection (SQL/NoSQL/command/path/RCE):**
- [ ] No string-built queries: all queries parameterized/ORM-bound
- [ ] No command injection (`exec(`/`os.system(`/`shell=True`/backticks with variable input)
- [ ] No path traversal on file reads/writes; **no arbitrary file download** — download/serve endpoints validate the requested path against an allowed set, never trust a raw path/ID from the request
- [ ] No code executed from user-controlled input (templates, config values, `eval`, dynamic imports) without strict allowlisting

**Authentication & access control:**
- [ ] Every endpoint has a defined auth requirement (public vs authenticated vs role-gated), checked against an actual route inventory — not assumed
- [ ] Passwords hashed with bcrypt/argon2/scrypt; sessions/JWT have expiry, verified signature, no `alg:none`, tokens not in localStorage; cookies HttpOnly+Secure+SameSite
- [ ] **Session fixation:** session ID/token is regenerated on login, never reused from a pre-auth state
- [ ] **Logout invalidation:** logout revokes the session/token server-side, not just client-side deletion
- [ ] **Token generation:** all security tokens (session IDs, password-reset tokens, API keys) generated via a cryptographically secure random source — never `Math.random()`, sequential IDs, or timestamp-based values
- [ ] Every state-changing endpoint checks authorization; object IDs from requests checked for ownership (IDOR)
- [ ] Rate limiting + lockout/backoff on login, signup, password-reset
- [ ] **Public admin interfaces:** admin/internal panels require auth, are not linked from public pages or listed in any public sitemap/robots file, and ideally sit behind an additional network restriction
- [ ] **Credential reuse across environments:** dev/staging/production use distinct credentials and secrets — verified different, not just "supposed to be"

**Cryptography & secrets:**
- [ ] Repo + full history clean of keys/tokens/passwords
- [ ] TLS everywhere; no disabled certificate verification; sensitive data encrypted at rest where required

**XSS & frontend:**
- [ ] Every `innerHTML`/`dangerouslySetInnerHTML`/`v-html`/unescaped template output reviewed; user content escaped or sanitized
- [ ] Security headers set (CSP, HSTS, X-Content-Type-Options, frame-ancestors)
- [ ] CSRF protection on cookie-authenticated state-changing routes

**Misconfiguration & infrastructure:**
- [ ] `DEBUG=False` in production; no stack traces to users; no default credentials
- [ ] CORS: no wildcard origin with credentials
- [ ] Docker/infra: non-root, pinned images, DB ports not public, no privileged containers, SSH by key only
- [ ] No unsafe deserialization (`pickle.loads`, unsafe `yaml.load`, `unserialize` on external data)
- [ ] SSRF: outbound requests never take a raw user-supplied URL without an allowlist
- [ ] File uploads: type + size validated, stored outside webroot, never executed

**Webhooks & integrations:**
- [ ] Every incoming webhook handler verifies a signature (HMAC or provider-equivalent) before processing the payload
- [ ] Outbound webhook secrets are rotatable without a code change

## Objectives — supply chain & dependency integrity (elevated for AI-written code)

- [ ] **Dependency confusion:** internal/private package names cannot be shadowed by a same-named public registry package (scoped packages or a private registry index used)
- [ ] **Typosquatting review:** every newly added dependency was checked against its expected name before install (one-character typos in popular package names are a known attack)
- [ ] 🔴 **Hallucinated-dependency check:** every import/require across the codebase resolves to a REAL, existing package in the actual registry — checked, not assumed. This specifically guards against AI coding agents inventing plausible-sounding package names that don't exist; attackers pre-register exactly these names ("slopsquatting") so an agent's hallucinated `import` can silently install malicious code. For each dependency: confirm it exists, has a plausible publish history (age, download count, maintainer), and matches what the code actually needed.
- [ ] Lockfile committed and dependencies pinned; no CI step blindly runs an "install latest" for anything security-relevant
- [ ] Legacy/deprecated code paths and dependencies are inventoried, not left silently reachable in production

## Objectives — AI-specific risk (applies whenever the product itself uses an LLM, embeds an AI agent, or calls a model API — mark N/A with a reason if it does not)

- [ ] **Prompt injection:** any untrusted content (user input, fetched web pages, uploaded documents, third-party API responses) that reaches a model prompt is treated as DATA, never as instructions the model should follow. Tested with at least one adversarial injection payload.
- [ ] **Indirect prompt injection:** content pulled from external sources (search results, emails, scraped pages) that the model reads is isolated from anything that can trigger a tool call — an attacker-controlled document must not be able to make the AI take an action.
- [ ] **Tool-calling abuse / least privilege:** every tool or function the AI can invoke is scoped to the minimum it needs; there is no blanket shell, filesystem, or database access. Destructive tool calls (delete, send, pay, deploy) require explicit confirmation or are disabled outside a controlled context.
- [ ] **Sandboxing:** any AI-executed code runs in an isolated, resource-limited environment — never directly against the host or production credentials.
- [ ] **Output handling:** AI-generated output shown to users is treated as untrusted input (same XSS/injection rules apply) and is never executed as code without human review.
- [ ] **Data exfiltration via AI output:** the model cannot be prompted into revealing secrets, other users' data, or its own system prompt/instructions — tested with at least one adversarial "reveal your instructions / reveal other data" attempt.
- [ ] **Training/inference data leakage:** no customer data is used to fine-tune or improve a shared/third-party model without explicit consent; the model provider's data-retention and training-use policy has been reviewed and recorded.

## Objectives — data protection & wrap-up

- [ ] No secrets/PII in logs; auth events (login, failure, permission denial) ARE logged
- [ ] If personal data is handled: GDPR-style duties checked — what's collected, why, deletion on request; privacy policy exists
- [ ] Every finding triaged in `docs/security-review.md`: fixed, or risk accepted in writing BY THE OWNER
- [ ] Credentials inventory: every token/DB user/cloud role listed with least-privilege confirmation

## Verification — the agent MUST verify

- [ ] All four scanner types present in CI config and green/triaged 🔴
- [ ] Full-history secret scan result recorded; any leaked secret rotated 🔴
- [ ] Code-hunt sections above each recorded as pass / findings-fixed / risk-accepted-by-owner in `docs/security-review.md` 🔴
- [ ] Zero unaddressed critical/high findings across all scanners and the hunt 🔴
- [ ] Every dependency in the manifest verified to resolve to a real, actively-maintained package — hallucination/typosquat check recorded 🔴
- [ ] If AI features are present: prompt-injection test executed with an adversarial payload; tool permissions reviewed and scoped; sandboxing confirmed 🔴 (skip only with an explicit "no AI features" note)
- [ ] Staging response headers checked (curl -I) for the security header set
- [ ] Session fixation, logout invalidation, and token-generation checks each recorded pass/fail

## Evidence

```
CI workflow security steps     scanner outputs
docs/security-review.md (findings table + credentials inventory + owner signatures)
dependency verification log (each import checked against registry)
AI red-team test transcript (if applicable)
curl -I staging-url
```

## Failure Conditions

- Any confirmed critical finding "parked for later"
- Scanners installed but set to warn-only with no triage process
- Risk accepted by anyone other than the owner
- "No findings" reported without evidence of the hunt actually running
- A dependency name was never checked against the real registry (the agent trusted its own suggestion)
- AI tool access includes unscoped shell/filesystem/database privileges
- Admin panel reachable without authentication or discoverable via a public sitemap

## Completion Criteria

- [ ] All objectives verified; all 🔴 pass
- [ ] Security checks now run on every PR permanently
- [ ] State + dashboard updated
