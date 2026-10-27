# .ai-governance — Self-Auditing Project Operating System

This directory is the operating system of this software project. It is written for AI coding agents (Claude Code, Codex, Gemini CLI, Amp, Cursor, Roo Code…) and humans alike.

**Agents: read `07_AI_AGENT_RULES.md` first. It is your constitution. Nothing here may be marked complete without verified evidence.**

## Map

| File | Purpose |
|---|---|
| `01_PROJECT_STATE.md` | Current phase, progress, blockers, next action — the single source of truth |
| `02_MASTER_CHECKLIST.md` | One-page view of all 13 phases and their gates |
| `03_PHASES/` | Executable phase files: Objectives → Verification → Evidence → Failure Conditions → Completion Criteria |
| `04_DELIVERABLES/` | Deliverable matrix, evidence requirements, audit procedure + report log |
| `05_RUNBOOKS/` | Exact commands to run, test, deploy, monitor, and secure each subsystem |
| `06_TEMPLATES/` | ADR, Charter, Sprint Review, Postmortem, DoD, WoW |
| `07_AI_AGENT_RULES.md` | Agent constitution: NEVER/ALWAYS rules, verification & marking protocol |
| `EXECUTIVE_DASHBOARD.md` | Auto-regenerated plain-English status for non-technical owners |

## Lifecycle

Phases 01–12 run **once**, in order, gated — including a dedicated Reliability & Data Integrity phase (06) and an expanded Security phase (11) covering supply-chain, dependency-hallucination, and AI-specific risk. Phase 13 (Continuous Improvement) is the permanent mode afterwards — recurring loops, no phases. Reality outranks records: the current phase is the first phase whose Verification fails against the actual repository.

## The one rule

🔴 gates can never be skipped — not by the agent, not by the owner. Everything else requires owner approval to skip and is recorded.
