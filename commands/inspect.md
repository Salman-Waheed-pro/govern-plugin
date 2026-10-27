---
description: Read-only project inspection — completeness of all lifecycle deliverables + OWASP-level security sweep, with graded report
---

Read the file `${CLAUDE_PLUGIN_ROOT}/skills/inspect/SKILL.md` and follow it exactly, treating the current working directory as the project under inspection. Interpret arguments:

- `$ARGUMENTS` empty → full inspection (completeness + quality + security + scalability/modularity).
- `$ARGUMENTS` = `security` → security sweep only, at maximum depth.
- `$ARGUMENTS` = `completeness` → deliverable/structure audit only.
- `$ARGUMENTS` = `quick` → structure scan + high-signal security greps only; no builds or test runs.

This command is an INSPECTOR: it must not modify project code. The only files it may write are the audit report and, if present, `.ai-governance/04_DELIVERABLES/PROJECT_AUDIT.md` and `EXECUTIVE_DASHBOARD.md`.
