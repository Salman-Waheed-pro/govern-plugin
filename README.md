# govern — SDLC Governance Plugin for Claude Code

Turns any coding agent into a governed delivery machine. Two commands:

- **`/govern`** — installs (if missing) and operates the project's `.ai-governance/` system: detects the current phase, verifies previous deliverables against the actual repository, executes the current phase checklist, strikes through verified items with evidence, and updates `01_PROJECT_STATE.md` + `EXECUTIVE_DASHBOARD.md`.
- **`/inspect`** — read-only auditor: reviews the whole project for **completeness** (every lifecycle deliverable) and **security** (OWASP Top 10 sweep: SQL injection, XSS, secrets, auth flaws, misconfiguration — frontend, backend, and infrastructure), then writes a graded audit report.

## Install

```bash
# From a local clone
claude plugin marketplace add /path/to/govern-plugin
claude plugin install govern@govern-marketplace

# Or straight from GitHub:
claude plugin marketplace add Salman-Waheed-pro/govern-plugin
claude plugin install govern@govern-marketplace
```

## What gets installed into a project

Running `/govern` in a project without governance copies `templates/ai-governance/` into the project as `.ai-governance/`:

- `00_README.md` — orientation
- `01_PROJECT_STATE.md` — current phase, progress, blockers, next action (agent-maintained)
- `02_MASTER_CHECKLIST.md` — one-page view of all phases and gates
- `03_PHASES/` — 13 executable phase files (Kickoff → Continuous Improvement), including a dedicated Reliability & Data Integrity phase and an expanded Security phase covering supply-chain, dependency-hallucination, and AI-specific risk. Each phase has Objectives, Verification, Evidence, Failure Conditions, Completion Criteria
- `04_DELIVERABLES/` — deliverable matrix, evidence rules, audit procedure + report log
- `05_RUNBOOKS/` — exact commands: Docker, GitHub Actions, Kubernetes, Monitoring, Testing, Security tooling
- `06_TEMPLATES/` — ADR, Project Charter, Sprint Review, Postmortem, DoD, WoW
- `07_AI_AGENT_RULES.md` — the agent's constitution (NEVER/ALWAYS rules, marking protocol)
- `EXECUTIVE_DASHBOARD.md` — auto-regenerated plain-English status for non-technical owners

Works with Claude Code natively; the `.ai-governance/` directory itself is plain markdown, so Codex, Gemini CLI, Amp, Cursor, and Roo Code can follow it too (point their rules file at `07_AI_AGENT_RULES.md`).


## Related

[sdlc-governance-system](https://github.com/Salman-Waheed-pro/sdlc-governance-system) is the earlier, tool-agnostic version of the same idea: plain markdown phases any agent or team can follow. This plugin is its Claude Code packaging, extended to 13 phases with a separate read-only `/inspect` auditor.

## License

MIT
