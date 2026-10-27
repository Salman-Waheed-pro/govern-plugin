# Phase 12 — Incidents

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** even well-built systems fail. Prepare the response BEFORE the first real fire: who leads, what happens first (rollback), how users hear about it, and how the team learns without blame.

---

## Objectives

- [ ] Incident response procedure adopted → `docs/incident-response.md`: declare → stabilize (rollback first!) → communicate → timestamp everything → confirm recovery
- [ ] Hard rules documented: no live-editing production files; hotfixes go through the pipeline (reviewed, even at 2am); never blame a person; data-exposure → owner informed immediately + legal notification duties checked (e.g. GDPR 72h)
- [ ] Blameless postmortem template ready (`06_TEMPLATES/Postmortem.md`), filed under `docs/postmortems/`
- [ ] Severity levels + response expectations defined (S1 all-hands vs S3 next-day)
- [ ] Status-communication plan: where users are informed if impact >15 min
- [ ] **Incident drill executed**, informed by the failure modes surfaced in Phase 06 (Reliability) and Phase 11 (Security) — e.g. simulate a dependency outage, a race-condition data conflict, or a credential leak, not just "the server is down": simulate on staging → follow the procedure end-to-end → timeline kept → postmortem written → action items on the board

## Verification — the agent MUST verify

- [ ] `exists: docs/incident-response.md` covering declare/stabilize/communicate/recover
- [ ] `exists: docs/postmortems/` with ≥1 drill postmortem containing timeline, root cause (asked "why" past the first answer), action items with owners 🔴
- [ ] Drill action items exist on the Kanban board 🔴
- [ ] Rollback used during the drill matches `docs/operations.md` and worked

## Evidence

```
docs/incident-response.md     docs/postmortems/YYYY-MM-DD-drill.md
board cards for action items     drill timeline
```

## Failure Conditions

- Postmortem names a person as root cause (the missing guardrail is the root cause)
- Drill skipped ("we'll learn in the real one")
- Action items written but never tracked
- Drill only covers "server down" and ignores data-integrity or security failure modes

## Completion Criteria

- [ ] All objectives verified; all 🔴 pass
- [ ] State + dashboard updated
