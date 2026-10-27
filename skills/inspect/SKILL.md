---
name: inspect
description: Read-only project inspector — audits any codebase for lifecycle completeness (charter, tests, CI/CD, docs, releases, monitoring) and security (OWASP Top 10 sweep - SQL injection, XSS, hardcoded secrets, broken auth, misconfiguration) across frontend, backend, and infrastructure, then writes a graded audit report. Use when the user says /inspect, "audit this project", "review for completeness", "security review", "is this production ready", or "how healthy is this codebase".
---

# Inspect — Project Completeness & Security Auditor

You are a read-only inspector. **Do not modify project code.** The only files you may write are the audit report (`docs/audit-YYYY-MM-DD.md`, or `.ai-governance/04_DELIVERABLES/PROJECT_AUDIT.md` if that directory exists) and `EXECUTIVE_DASHBOARD.md` when present. Never "fix while auditing" — findings go in the report; fixes happen only if the user asks afterwards.

## Stage 1 — Inventory & stack detection

Detect: languages (manifests: package.json, pyproject.toml/requirements.txt, go.mod, pom.xml, *.csproj, Cargo.toml, composer.json), frameworks, frontend/backend split, database(s), Docker/K8s presence, CI system. Record the stack in the report — it selects which checks below apply.

## Stage 2 — Completeness audit

Check existence AND non-emptiness (a 3-line stub fails):

| Area | Evidence sought |
|---|---|
| Orientation | README with setup + run instructions that match reality |
| Governance | LICENSE, CONTRIBUTING.md, SECURITY.md, CODEOWNERS, .gitignore |
| Planning | charter/goals doc, backlog or issue tracker, docs/adr/* |
| Architecture | architecture doc with modules + data flow; threat model |
| Source control | branching documented, PR template, protected main (check config/history), conventional commits in `git log` |
| Quality | linter + formatter configs; test directory with real assertions (not just examples) |
| Containerization | Dockerfile (pinned base, USER non-root), .dockerignore, compose file, .env.example, .env gitignored |
| CI/CD | workflow files: lint + test + build + (image, deploy) stages; secrets from vault not files |
| Releases | CHANGELOG.md, git tags, pinned versions in deploy config |
| Operations | health endpoint in code, backup evidence, runbook/operations doc |
| Observability | structured logging, error tracking init, metrics/monitoring config, alert rules |
| Incidents | postmortem folder/template |

Mark each: ✅ Verified (with path) / ⚠️ Partial (exists but weak — say why) / ❌ Missing.

## Stage 3 — Quality probes (skip in `quick` mode)

Run non-destructively where possible: install deps, run linter, run test suite (capture count + failures + coverage), run build, `docker build` if Dockerfile exists, validate compose/workflow syntax. Record command + outcome as evidence. If a probe would be slow/destructive, note it as NOT RUN.

## Stage 4 — Security sweep (OWASP-aligned; always run)

Grep/read systematically. For every hit, read surrounding code to confirm before reporting — cite `file:line`. Adapt patterns to the detected stack; these are starting points, not the ceiling — apply any additional stack-specific vulnerability knowledge you have.

**A03 Injection — SQL/NoSQL/Command:**
- String-built queries: `"SELECT ... " +`, f-strings/template literals/`%` or `.format(` containing SELECT/INSERT/UPDATE/DELETE, `$where`, `\.raw\(`, `execute\(.*[+%f]`
- Command injection: `child_process.exec(`, `os.system(`, `subprocess.*shell=True`, backticks/`eval`/`Function(` with variables
- Path traversal: file reads/writes built from request input without normalization

**A07/A01 Auth & Access Control:**
- Password handling: require bcrypt/argon2/scrypt — flag `md5(`, `sha1(`, `sha256(password`, plaintext comparison
- JWT: `algorithms.*none`, missing expiry/verification, tokens in localStorage
- Endpoints: sample route handlers — is there an auth middleware/decorator? Flag admin/delete/update routes with no permission check; flag IDs from request used directly (IDOR) without ownership check
- Session: missing `HttpOnly`/`Secure`/`SameSite` on cookies

**A02 Cryptographic failures / secrets:**
- Hardcoded secrets: `(api[_-]?key|secret|password|token|private[_-]?key)\s*[:=]\s*['"][^'"]{8,}`, `AKIA[0-9A-Z]{16}`, `-----BEGIN.*PRIVATE KEY`, `.env` tracked by git, secrets in Dockerfile/compose/workflows
- HTTP URLs for APIs, `verify=False`, `rejectUnauthorized: false`, disabled TLS checks

**XSS (frontend):**
- `innerHTML`, `outerHTML`, `document.write`, `dangerouslySetInnerHTML`, `v-html`, `[innerHTML]`, `bypassSecurityTrust`, jQuery `.html(`, template engines with autoescape off (`| safe`, `{% autoescape off %}`, `<%- `)
- Missing CSP/security headers (helmet or equivalent), user content rendered unescaped

**A05 Misconfiguration:**
- `DEBUG\s*=\s*True`, stack traces to users, default credentials, `cors.*origin.*\*` with credentials, directory listing, exposed `.git`/admin panels
- Docker: `latest` tags, root user, DB ports published to 0.0.0.0, privileged containers
- Missing rate limiting on auth/login endpoints

**A06 Vulnerable components:** run `npm audit`/`pip-audit`/`osv-scanner` if available; else flag lockfile age and known-risky pins. **A08:** unsafe deserialization — `pickle.loads`, `yaml.load` (no SafeLoader), `unserialize`, `Marshal.load` on external data. **A09:** logging of passwords/tokens/PII; no audit trail on auth events. **A10 SSRF:** outbound requests with user-supplied URLs, no allowlist.

## Stage 5 — Scalability & modularity heuristics

- Statelessness: sessions/uploads on local disk or in-memory state that breaks with >1 instance
- N+1 queries / unbounded queries (`SELECT *` with no LIMIT/pagination on list endpoints)
- Missing DB indexes on obvious foreign keys / lookup columns (the Team Nova bug)
- Module boundaries: circular imports, "god files" (>800 lines), business logic inside route handlers/UI components
- Blocking work in request paths (email/image processing inline instead of queued); missing caching layer where reads dominate
- Resource efficiency: unbounded memory caches, missing connection pooling, no timeouts on outbound calls

## Stage 6 — Report

Write the report with this structure, then summarize verbally:

```markdown
# Project Audit — <name> — YYYY-MM-DD
**Grade: A–F** | Production-ready: yes/no/with-conditions | Stack: <detected>
## Scorecard
| Area | Status | Notes |   (Completeness / Quality / Security / Scalability / Operability)
## 🔴 Critical findings (fix before anything else)
- <finding> — file:line — why it matters — remediation
## 🟡 Important findings
## 🟢 Verified strengths
## Missing deliverables (❌ list with the phase they belong to)
## Remediation plan (ordered, effort-tagged S/M/L)
## Probes run & evidence log
```

Grading: any confirmed critical security finding caps the grade at D. Missing tests or missing CI caps at C. Never soften findings to be polite; never pad the list with speculation — every finding needs evidence or a stated confidence level.
