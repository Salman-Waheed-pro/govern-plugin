# Phase 13 — Continuous Improvement (permanent mode)

Status:
- [ ] Not Activated
- [ ] Active (project graduated)

**Plain English:** the lifecycle is finished — forever. From now on the project runs on recurring LOOPS, not phases. This file IS the new system. When this phase activates, set `Mode: continuous` in `01_PROJECT_STATE.md`; phases 01–12 never run again for this product.

**Activation criteria:** phases 01–12 all ✅ with gates verified.

---

## Loop 1 — Per change (every piece of work)

- [ ] Board card with acceptance criteria exists before code starts
- [ ] Branch → conventional commits → PR ≤ ~300 lines
- [ ] 🔴 Review + green CI (lint, tests, security scans, dependency-hallucination check on any new import) before merge — no exceptions, including hotfixes
- [ ] Docs updated in the same PR if behavior changed
- [ ] Auto-deploy to staging + smoke test green; DoD met before card closes

## Loop 2 — Per release

- [ ] SemVer bump; CHANGELOG + human-readable release notes
- [ ] Annotated tag → pipeline publishes pinned image
- [ ] 🔴 Production deploy only from a tagged, staging-tested image, with approval
- [ ] Post-deploy smoke test; watch error dashboard 30 min; risky features behind flags/canary
- [ ] License audit re-run if dependencies changed

## Loop 3 — Weekly (owner + team, ~30 min)

- [ ] Board review: anything In Progress >5 days → ask why, split or unblock
- [ ] Dashboard review: error rate, latency, uptime vs last week
- [ ] Security alerts: criticals fixed within the week
- [ ] Demo what shipped; owner reorders backlog

## Loop 4 — Monthly

- [ ] Blameless retrospective → `docs/retros/` (keep/stop/try, follow up last month's "try")
- [ ] Dependency updates merged; coverage vs threshold checked
- [ ] 🔴 Backup restore drill (scratch env, data verified) — date recorded below
- [ ] Cost review vs Phase 02 budget; investigate creep
- [ ] DORA check: deploy frequency, lead time, change-failure rate, time-to-recover — trending?
- [ ] Prune: dead code, unused deps, stale flags, stale/legacy code paths, stale docs
- [ ] Re-run a lightweight load test if traffic has grown meaningfully since the last one

## Loop 5 — Per incident

→ `docs/incident-response.md`. Every incident ends with a blameless postmortem; the agent verifies within a week that its action items are on the board.

---

## When does the phase system ever return?

Only for a **new product**, or a change so large it invalidates the architecture (rewrite, new platform, new compliance regime, adding AI/agent features to a product that didn't have them — re-run Phase 11's AI-specific section) — then run a mini-lifecycle (charter → ADRs → gates) inside continuous mode.

## Loop log (agent-maintained)

| Loop | Last run | By | Notes |
|---|---|---|---|
| Weekly | — | — | — |
| Monthly | — | — | — |
| Restore drill | — | — | — |
| Load test | — | — | — |
