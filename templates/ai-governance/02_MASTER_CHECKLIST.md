# MASTER CHECKLIST — all phases at a glance

> Statuses mirror `01_PROJECT_STATE.md`. Details and verification live in `03_PHASES/`.
> 🔴 = gate, can never be skipped.

## Phase 01 — Kickoff
- [ ] Charter written and 🔴 approved by owner
- [ ] Team roster + roles (humans and AI agents)
- [ ] Ways of Working + 🔴 Definition of Done (requires review + tests)
- [ ] Repository + communication channel established

## Phase 02 — Planning & Architecture
- [ ] User stories with acceptance criteria; 🔴 MVP ≤ 7 stories with one stated assumption
- [ ] Kanban board live; 🔴 all work visible on it
- [ ] Architecture doc + data model; 🔴 module boundaries explicit
- [ ] 🔴 ≥3 ADRs; 🔴 threat model; 🔴 monthly cost ceiling written
- [ ] Data classification (sensitive/personal fields), license strategy, regulatory obligations named

## Phase 03 — Source Control & Code Quality
- [ ] Branching strategy chosen + documented (ADR)
- [ ] 🔴 Main protected: PR + review + green CI required
- [ ] Conventional commits, PR template, review guidelines
- [ ] Linter + formatter configured; 🔴 CI blocks on failure
- [ ] 🔴 No secrets in repo or history
- [ ] Dependency-add discipline (verify real, non-typosquatted packages); endpoint inventory maintained (no shadow routes)

## Phase 04 — Docker & Containerization
- [ ] Dockerfile (pinned base, non-root) + .dockerignore
- [ ] 🔴 `docker compose up` starts the full stack from clean checkout
- [ ] 🔴 No secrets baked into images; config via env vars
- [ ] 🔴 Data survives container recreation (tested)

## Phase 05 — Testing
- [ ] Unit + integration + API tests; e2e for core journey
- [ ] 🔴 CI blocks merge on test failure (demonstrated)
- [ ] 🔴 Coverage threshold agreed and auto-enforced
- [ ] 🔴 Idempotency test for the core state-changing operation; test isolation; 🔴 no production data/PII in any test fixture
- [ ] Lightweight chaos check executed; load-test harness scheduled before go-live

## Phase 06 — Reliability & Data Integrity
- [ ] 🔴 Transaction boundaries reviewed for top state-changing flows
- [ ] 🔴 No unbounded retry loops; retries bounded with backoff
- [ ] Migration safety reviewed (dry-run, rollback path); destructive ops require confirmation/soft-delete
- [ ] Dead-letter queues for async jobs; orphaned-resource cleanup
- [ ] Race conditions / TOCTOU reviewed on shared-state paths
- [ ] Timeouts on all outbound calls; bounded connection pools; no unbounded caches; pagination enforced
- [ ] Feature flags owned + expiring; 🔴 kill switch demonstrated
- [ ] 🔴 RTO/RPO defined and matched against actual backup cadence

## Phase 07 — CI/CD
- [ ] Pipeline: lint → test → build → image → registry (SHA-tagged)
- [ ] 🔴 Auto-deploy to staging on merge + smoke tests
- [ ] 🔴 Rollback rehearsed; 🔴 zero credentials in workflow files
- [ ] 🔴 Third-party CI actions pinned to commit SHA; no secrets leaked into CI logs

## Phase 08 — Releases & Production Readiness
- [ ] SemVer + annotated tags + CHANGELOG.md
- [ ] Feature-flag mechanism; env separation (dev/staging/prod)
- [ ] 🔴 Owner-signed production readiness checklist
- [ ] 🔴 Production runs pinned tested version; 🔴 prod rollback rehearsed
- [ ] Canary/staged rollout (or N/A recorded); kill switch confirmed reachable in production
- [ ] 🔴 License audit clean; compliance reporting duties named

## Phase 09 — Operations
- [ ] Health endpoint + restart policies
- [ ] Scheduled backups; 🔴 restore actually tested; 🔴 cadence matches RTO/RPO
- [ ] 🔴 Runbook exists and was followed by a second person

## Phase 10 — Observability
- [ ] Structured logs (no secrets/PII) retained beyond containers
- [ ] Error tracking + metrics + dashboard
- [ ] 🔴 Alert fired and reached a human in a drill
- [ ] Anomaly/abuse alerting configured

## Phase 11 — Security, Supply Chain & AI Risk
- [ ] Dependency, secret, SAST, and image scanning 🔴 wired into CI permanently
- [ ] 🔴 OWASP sweep done (injection, XSS, auth, crypto, misconfig, RCE, arbitrary file download) — findings triaged
- [ ] Session fixation, logout invalidation, secure token generation, public admin-interface exposure checked
- [ ] Webhook signature verification present on all incoming webhook handlers
- [ ] 🔴 Dependency confusion / typosquatting / **hallucinated-dependency check** — every import verified to resolve to a real package
- [ ] 🔴 If AI features present: prompt-injection test, tool-permission scoping, sandboxing, data-exfiltration test all pass
- [ ] 🔴 Zero unaddressed critical/high vulns; 🔴 full-history secret scan clean
- [ ] Auth review: hashing, expiry, per-endpoint authorization, rate limiting

## Phase 12 — Incidents
- [ ] Incident response procedure adopted
- [ ] 🔴 One incident drill executed (informed by Phase 06/11 failure modes) with timeline + blameless postmortem

## Phase 13 — Continuous Improvement (permanent)
- [ ] Mode switched to `continuous` in PROJECT_STATE
- [ ] Loops running: per-change, per-release, weekly, monthly, per-incident
