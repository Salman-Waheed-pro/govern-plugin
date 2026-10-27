# AI AGENT RULES — the constitution

Applies to every AI agent (and human) operating this project. These rules outrank convenience, speed, and politeness. Only the human owner may amend this file, in writing.

## NEVER

1. **Never mark a task complete without verification.** Evidence = file exists + has real content, command exited 0, or pattern confirmed present/absent. Claims — including your own memory — are not evidence.
2. **Never assume files exist.** Inspect the repository every session: files, CI config, docs, commits, tests, Docker, release notes, changelog, branches.
3. **Never skip a 🔴 gate.** Not for deadlines, not on owner instruction. A gate that cannot pass makes the phase ⛔ blocked, reported in plain English.
4. **Never work ahead of an unpassed gate** (e.g. no production deploy before Phase 10 security gates pass).
5. **Never delete or reword completed checklist history.** Struck-through lines are the audit trail.
6. **Never put secrets in code, images, workflows, logs, or these governance files.**
7. **Never fix silently during an audit.** Findings go in the report; fixes are separate, authorized work.
8. **Never report success that wasn't observed.** Failing tests, skipped steps, and partial work are reported as exactly that.

## ALWAYS

1. **Always boot from state:** read `01_PROJECT_STATE.md`, then verify the recorded phase against reality (`03_PHASES/*` Verification sections). Reality wins; record discrepancies in the Journal.
2. **Always verify the previous phase's gates still pass** (spot-check) before working — deliverables regress.
3. **Always capture evidence when completing an item** and write it inline:
   `- [x] ~~Objective~~ ✅ YYYY-MM-DD — evidence: <path | command → result>`
   Blocked: `- [!] Objective ⚠️ BLOCKED: <reason> — needs: <what/who>`
   Owner-approved skip (gates excluded): `- [~] ~~Objective~~ ⏭️ SKIPPED — approved: <owner>, <date>, reason`
4. **Always update after every task:** phase file markings → `01_PROJECT_STATE.md` (progress, blockers, Next Action, Journal) → `02_MASTER_CHECKLIST.md` → `EXECUTIVE_DASHBOARD.md`.
5. **Always strike through a phase title only when its Completion Criteria all pass**, then advance `Current Phase`.
6. **Always provide the exact commands** to run, test, validate, deploy, and troubleshoot anything you implement — add them to the matching file in `05_RUNBOOKS/`.
7. **Always end a session with a plain-English status** (3–5 sentences) a non-technical owner understands, in the Journal and to the user.
8. **Always ask the owner first** before: deleting data, spending money, deploying to production, granting access, or skipping any item.
9. **Always generate an audit report** in `04_DELIVERABLES/PROJECT_AUDIT.md` when asked to audit, following its procedure — including gaps in documentation and implementation, not just code.

## Verification vocabulary

| Rule | Meaning |
|---|---|
| `exists: <glob>` | file(s) exist AND are non-trivial (a stub fails) |
| `contains: <file> "<pattern>"` | pattern present |
| `absent: "<pattern>" in <glob>` | pattern NOT present |
| `passes: <command>` | command exits 0 (adapt to the project's stack; record the actual command run) |
| `ci: <check>` | verified in CI config and at least one run |
| `human: <who>` | named human sign-off recorded in the file |

## Session output contract

Every governance session ends with: current phase + %, what was verified, what was completed (with evidence), what is blocked, the single Next Action.
