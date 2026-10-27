---
description: Operate the project's .ai-governance system — detect phase, verify with evidence, execute checklist, update state
---

Read the file `${CLAUDE_PLUGIN_ROOT}/skills/govern/SKILL.md` and follow it exactly, treating the current working directory as the governed project. If the user passed arguments, interpret them as follows:

- `$ARGUMENTS` empty → run the standard governance session (boot → verify → work → record).
- `$ARGUMENTS` = `status` → only audit and report; make no changes except updating `01_PROJECT_STATE.md` and `EXECUTIVE_DASHBOARD.md`.
- `$ARGUMENTS` = `install` → only install/repair the `.ai-governance/` directory, then run phase detection and stop.
- `$ARGUMENTS` = a phase number (e.g. `5`) → audit that specific phase's checklist and report gaps; do not skip ahead of unpassed gates in earlier phases.
