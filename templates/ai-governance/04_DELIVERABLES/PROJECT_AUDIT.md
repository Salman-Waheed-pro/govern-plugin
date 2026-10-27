# PROJECT AUDIT

## How to run a full audit (agent procedure)

1. **Inventory:** detect stack, list top-level structure, find manifests/CI/Docker/docs.
2. **Completeness:** walk `DELIVERABLE_MATRIX.md` row by row; update every Status with evidence per `EVIDENCE_REQUIREMENTS.md`.
3. **Quality probes:** run lint, tests (+coverage), build, `docker compose config` — non-destructively; record outcomes.
4. **Security sweep:** execute the full hunt in `03_PHASES/PHASE_10_SECURITY.md` (OWASP sections) regardless of current phase — early warnings are cheap.
5. **Scalability/modularity heuristics:** stateless? N+1 queries? missing indexes? god-files? business logic in route handlers? unbounded caches? missing timeouts?
6. **Report:** append a dated report below (newest first). Update `EXECUTIVE_DASHBOARD.md` and `01_PROJECT_STATE.md`.

Audits are read-only for project code. Findings become board cards, not silent fixes.

## Report format

```markdown
## Audit — YYYY-MM-DD — by <agent/human>
**Grade:** A–F | **Production-ready:** yes / no / with conditions | **Phase (verified):** NN
### Scorecard
| Completeness | Quality | Security | Scalability | Operability |
|---|---|---|---|---|
| x/10 | x/10 | x/10 | x/10 | x/10 |
### 🔴 Critical (fix first)
- <finding> — file:line — impact — remediation
### 🟡 Important
### 🟢 Verified strengths
### ❌ Missing deliverables (with phase)
### Remediation plan (ordered, S/M/L effort)
### Probes run
```

Grading rules: confirmed critical security finding → max D. No tests or no CI → max C. Every finding cites evidence or states confidence.

---

# Audit log

- (no audits yet)
