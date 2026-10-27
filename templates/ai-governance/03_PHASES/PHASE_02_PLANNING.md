# Phase 02 — Planning & Architecture

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** decide WHAT to build first (small!) and HOW it will be built — with decisions written down so they can be challenged later.

---

## Objectives — Planning

- [ ] User stories written (`As a <user> I want <goal> so that <benefit>`), each with acceptance criteria → `docs/backlog.md` or issue tracker
- [ ] MVP defined: 3–7 stories marked, plus ONE sentence naming the assumption the MVP tests
- [ ] Backlog prioritized as an ordered list; rough roadmap (MVP → next 2 increments)
- [ ] Kanban board created (Backlog / Ready / In Progress / In Review / Done) with WIP limit; URL recorded in backlog doc
- [ ] Owner reviewed and reordered priorities (sign-off line)

## Objectives — Architecture

- [ ] `docs/architecture.md`: components, responsibilities, data-flow diagram (mermaid/ASCII fine)
- [ ] Module boundaries explicit: each module's single responsibility + public interface (this is what makes the product modular)
- [ ] Data model: entities, key fields, relationships, **indexes for known lookups**
- [ ] Tech stack chosen; ADR per significant choice (stack, database, auth, hosting) from `06_TEMPLATES/ADR.md` → `docs/adr/`
- [ ] Threat model `docs/threat-model.md`: sensitive data, attacker profiles, top 5 risks + mitigations
- [ ] Budgets in architecture doc: expected launch load, response-time target, monthly cost ceiling; scalability note (what breaks at 10×, planned response — stateless services, caching, DB scaling path)

## Objectives — legal, compliance & data classification (decided before code, not after)

- [ ] Data classification: which fields are personal/sensitive data is decided now, recorded in the threat model, so Phase 05 (test data), Phase 09 (backups), and Phase 11 (security) all inherit the same list
- [ ] License strategy decided: what license the project itself uses, and the policy for what dependency licenses are acceptable (feeds the Phase 08 license audit)
- [ ] Any regulatory obligations identified up front (GDPR/CCPA, accessibility, industry-specific) with an owner named

## Verification — the agent MUST verify

- [ ] `exists: docs/backlog.md` (or tracker) with ≥8 stories; MVP section lists ≤7 🔴
- [ ] Every MVP story has ≥1 checkable acceptance criterion 🔴
- [ ] Board URL recorded 🔴
- [ ] `exists: docs/architecture.md` with a diagram AND a "Modules" section (≥2 modules with interfaces) 🔴
- [ ] `exists: docs/adr/` with ≥3 ADRs each containing "Consequences" 🔴
- [ ] `exists: docs/threat-model.md` naming sensitive data 🔴
- [ ] Architecture doc contains a cost figure and a latency target 🔴

## Evidence

```
docs/backlog.md   docs/architecture.md   docs/adr/*.md   docs/threat-model.md
Kanban board URL
```

## Failure Conditions

- MVP > 7 stories or no testable assumption → it's not an MVP, cut it
- "We'll figure out modules in code" → fails modularity gate
- No cost ceiling, no threat model, or ADRs missing consequences
- Stories without acceptance criteria

## Completion Criteria

- [ ] All objectives verified; all 🔴 items above pass
- [ ] Owner sign-off on priorities recorded
- [ ] State + dashboard updated
