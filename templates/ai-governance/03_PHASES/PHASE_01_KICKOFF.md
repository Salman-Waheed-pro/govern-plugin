# Phase 01 — Kickoff

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** agree on WHY the project exists, WHO does it, and HOW you work together — before any code.

---

## Objectives

- [ ] Project charter written from `06_TEMPLATES/ProjectCharter.md` → `docs/charter.md` (problem, goal, scope, OUT of scope, stakeholders, measurable success criteria, constraints incl. budget)
- [ ] Team roster with roles — humans AND AI agents; who owns product decisions, who owns technical decisions
- [ ] Ways of Working written from `06_TEMPLATES/WoW.md` → `docs/ways-of-working.md` (cadence, channels, decision rule)
- [ ] Definition of Done written from `06_TEMPLATES/DoD.md` → `docs/definition-of-done.md`
- [ ] Repository created with README stub, .gitignore for the stack, LICENSE decision
- [ ] Communication channel established and recorded in WoW

## Verification — the agent MUST verify

- [ ] `exists: docs/charter.md` and it `contains: "Out of scope"` and `contains: "Success criteria"` with measurable statements
- [ ] `contains: docs/charter.md "Approved by"` with a name and date 🔴
- [ ] `exists: docs/ways-of-working.md` naming a channel
- [ ] `exists: docs/definition-of-done.md` and it `contains: "review"` AND `contains: "test"` 🔴
- [ ] `exists: README.md, .gitignore`; repository has an initial commit

## Evidence

```
docs/charter.md          docs/ways-of-working.md
docs/definition-of-done.md
git log --oneline | tail -1
```

## Failure Conditions — do NOT mark complete if

- Charter has no owner approval line, or success criteria are vague ("app is good")
- Out-of-scope list is empty
- DoD does not require code review and automated tests
- Roster doesn't say who decides product vs technical questions

## Completion Criteria

- [ ] All objectives verified with evidence
- [ ] 🔴 G1: Owner approval recorded in charter
- [ ] 🔴 G2: Success criteria measurable by a non-technical person
- [ ] 🔴 G3: DoD requires review + tests
- [ ] `01_PROJECT_STATE.md` and `EXECUTIVE_DASHBOARD.md` updated
