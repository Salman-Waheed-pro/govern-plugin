# DELIVERABLE MATRIX

> The complete inventory of what this project must produce. The agent updates Status only with evidence. ❌ Missing / ⚠️ Partial / ✅ Verified.

| Phase | Deliverable | Evidence location / command | Status |
|---|---|---|---|
| 01 | Project charter (owner-approved) | `docs/charter.md` | ❌ |
| 01 | Ways of Working | `docs/ways-of-working.md` | ❌ |
| 01 | Definition of Done | `docs/definition-of-done.md` | ❌ |
| 01 | Repo initialized | `README.md`, `.gitignore`, `git log` | ❌ |
| 02 | Backlog + user stories + MVP | `docs/backlog.md` / tracker | ❌ |
| 02 | Kanban board | URL in backlog doc | ❌ |
| 02 | Architecture doc + modules + data model | `docs/architecture.md` | ❌ |
| 02 | ADRs (≥3) | `docs/adr/*.md` | ❌ |
| 02 | Threat model + data classification | `docs/threat-model.md` | ❌ |
| 02 | License strategy + regulatory owner named | `docs/architecture.md` or charter | ❌ |
| 03 | Branching strategy ADR + protected main | ADR + repo settings | ❌ |
| 03 | CONTRIBUTING + PR template | `CONTRIBUTING.md`, `.github/PULL_REQUEST_TEMPLATE.md` | ❌ |
| 03 | Linter + formatter configs | config files + passing runs | ❌ |
| 03 | CI skeleton | `.github/workflows/*.yml` + green run | ❌ |
| 03 | Env config discipline | `.env.example`, `.env` gitignored | ❌ |
| 03 | Endpoint inventory (no shadow routes) | `docs/architecture.md` route table | ❌ |
| 04 | Dockerfile + .dockerignore | files + `docker build` | ❌ |
| 04 | Compose stack + volumes | `docker compose up/ps` + persistence test | ❌ |
| 05 | Test suite (unit/integration/API/e2e) | test dirs + suite run | ❌ |
| 05 | Coverage threshold enforced | coverage report + config | ❌ |
| 05 | Idempotency + concurrency tests | test names + results | ❌ |
| 05 | Test plan (incl. no-prod-data rule) | `docs/testing.md` | ❌ |
| 06 | Reliability review (transactions, retries, migrations) | `docs/reliability.md` | ❌ |
| 06 | Kill switch demonstrated | note in `docs/reliability.md` | ❌ |
| 06 | RTO/RPO defined | `docs/reliability.md` | ❌ |
| 07 | Full pipeline + registry images | workflows + registry | ❌ |
| 07 | Staging env + auto-deploy + smoke | `docs/environments.md` + run URL | ❌ |
| 07 | Rehearsed rollback | note in `docs/environments.md` | ❌ |
| 07 | Pipeline docs (incl. pinned CI actions) | `docs/pipeline.md` | ❌ |
| 08 | v1.0.0 tag + CHANGELOG | `git tag -n`, `CHANGELOG.md` | ❌ |
| 08 | Signed production readiness checklist | `docs/release-checklist-v1.md` | ❌ |
| 08 | License audit | `docs/license-audit.md` | ❌ |
| 08 | Production live, pinned version | prod URL + image tag | ❌ |
| 09 | Health endpoint + auto-restart | `curl /health` + kill test | ❌ |
| 09 | Backups + tested restore (matches RTO/RPO) | job config + drill note | ❌ |
| 09 | Operations runbook | `docs/operations.md` | ❌ |
| 10 | Structured logging (no PII) | log samples | ❌ |
| 10 | Error tracking + metrics + dashboard | tracker event + dashboard URL | ❌ |
| 10 | Alerting + fire drill + anomaly rule | alert config + drill note | ❌ |
| 11 | 4 scanner types in CI | workflow steps + outputs | ❌ |
| 11 | Security review (OWASP + supply-chain + AI hunt) | `docs/security-review.md` | ❌ |
| 11 | Dependency hallucination/typosquat check | `docs/security-review.md` dependency log | ❌ |
| 11 | AI-specific risk review (or N/A recorded) | `docs/security-review.md` | ❌ |
| 12 | Incident procedure + drill postmortem | `docs/incident-response.md`, `docs/postmortems/` | ❌ |
| 13 | Continuous mode active | `01_PROJECT_STATE.md` Mode + loop log | ❌ |
