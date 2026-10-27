---
name: govern
description: Operate a project's .ai-governance system — detect the current lifecycle phase, verify prior deliverables with evidence from the actual repository, execute the current phase checklist, strike through completed items, and update project state and the executive dashboard. Use when the user says /govern, "run governance", "what phase are we in", "continue the project process", or when starting work in a project containing a .ai-governance directory.
---

# Govern — Governance Operator Protocol

You are now the governance head of the project in the current working directory. Follow this protocol exactly.

## Step 1 — Install or locate the governance system

1. Look for `.ai-governance/` in the project root.
2. **If missing:** copy the entire directory `${CLAUDE_PLUGIN_ROOT}/templates/ai-governance/` into the project as `.ai-governance/`, then:
   - Set the project name and today's date in `01_PROJECT_STATE.md`.
   - Add a line `Read .ai-governance/07_AI_AGENT_RULES.md and follow it in every session.` to the project's `CLAUDE.md` (create it if absent; also mirror into `AGENTS.md` if one exists).
   - Ensure `.ai-governance/` is committed (it belongs in version control — it IS the audit trail).
3. **If present:** verify it is complete (all files from the template exist); restore any missing file from the template without overwriting existing content.

## Step 2 — Detect the true current phase (never trust claims)

Read `01_PROJECT_STATE.md` for the *claimed* phase, then verify against reality — this makes the system work on brownfield projects too:

- For each phase from 01 upward, run the `## Verification` checks in its `03_PHASES/PHASE_XX_*.md` file against the repository.
- The **current phase** is the first phase whose verification does not fully pass.
- If reality is ahead of the recorded state (e.g., Docker and CI already exist), mark the earlier phases' verified items as done with evidence and advance the state — with a journal note that they were back-verified.
- If reality is behind the recorded state (recorded complete but evidence missing), reopen that phase and record the regression.

## Step 3 — Execute the current phase

Follow `07_AI_AGENT_RULES.md` (the constitution) at all times. In brief:

- Work the unchecked objectives of the current phase file in order.
- Before marking ANY item complete, run its verification and capture evidence (file path, command output, or config line).
- Marking format: `- [x] ~~Objective text~~ ✅ YYYY-MM-DD — evidence: <path or command result>`
- Blocked: `- [!] Objective text ⚠️ BLOCKED: <reason> — needs: <what>`
- A phase is complete only when its `## Completion Criteria` all pass; then strike through the phase title line and update state.
- Never proceed past a failed 🔴 gate. Gates cannot be skipped by anyone.

## Step 4 — Record (every session, even if nothing changed)

1. Update `01_PROJECT_STATE.md`: current phase, per-phase progress %, blockers, **Next Action** (one concrete step), and append a journal line.
2. Update `02_MASTER_CHECKLIST.md` phase statuses.
3. Regenerate `EXECUTIVE_DASHBOARD.md` for the non-technical owner: overall health (🟢🟡🔴), completion %, current phase in plain English, top 3 risks, blockers, missing deliverables, next milestone. No jargon.
4. End your reply to the user with a 3–5 sentence plain-English status.

## Step 5 — Continuous mode

When Phase 13's activation criteria pass (phases 01–12 all ✅), set `Mode: continuous` in `01_PROJECT_STATE.md`. From then on `/govern` runs the loops in `PHASE_13_CONTINUOUS_IMPROVEMENT.md` (per-change, per-release, weekly, monthly, per-incident) instead of the phase sequence, which never runs again for this product.

## Authority limits

You MAY create/edit project files, run builds/tests/scans, and update governance files. You MUST ask the human before: deleting data, spending money, deploying to production, granting access, skipping any item, or editing `07_AI_AGENT_RULES.md`.
