# Phase 03 — Source Control & Code Quality

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** set up the workshop before building the furniture — from here on, no change enters the product without a second look and a passing automatic check.

---

## Objectives

- [ ] Branching strategy chosen and documented as an ADR. Default: Feature Branch Workflow; SaaS deploying often: GitHub Flow; staged environments: GitLab Flow; versioned/regulated: Git Flow; mature team: Trunk-Based
- [ ] Main branch protected: PRs required, ≥1 review, CI must pass, no direct pushes
- [ ] Conventional Commits adopted (`feat:` `fix:` `docs:` `refactor:` `test:` `ci:` `chore:`) → documented in `CONTRIBUTING.md`
- [ ] PR template with description/linked-issue/DoD checklist → `.github/PULL_REQUEST_TEMPLATE.md`
- [ ] Code review guidelines in CONTRIBUTING.md: correctness, readability, security, performance, tests; PRs ≤ ~300 lines; one feature per PR
- [ ] Linter configured for the stack (ESLint/Ruff/golangci-lint…), config committed
- [ ] Formatter configured (Prettier/Black/gofmt…) — style automated, never debated
- [ ] Project skeleton per architecture doc: folders match module boundaries, entry point runs, dependency manifest + lockfile
- [ ] CI skeleton: on push/PR → install → lint → format-check → (placeholder) test → see `05_RUNBOOKS/GitHubActions.md`
- [ ] `.env.example` committed; `.env` gitignored — config via environment variables from day one
- [ ] First change merged through the FULL flow: branch → conventional commit → PR → review → green CI → merge

## Objectives — dependency & endpoint hygiene

- [ ] **Dependency-add discipline:** adding a new dependency requires confirming it actually exists in the registry, matches the expected name (not a typosquat), and has a plausible maintenance history — before it's committed to the manifest. This applies to every dependency an AI agent proposes, not just human-picked ones.
- [ ] **Route/endpoint inventory** maintained (even a simple table in `docs/architecture.md`): every endpoint that exists in code is listed — no "shadow" endpoints that exist in the app but aren't documented or reviewed

## Verification — the agent MUST verify

- [ ] `exists: docs/adr/*branching*` (or equivalent ADR)
- [ ] Branch protection active (settings or a rejected direct-push attempt recorded) 🔴
- [ ] `exists: CONTRIBUTING.md` containing "Conventional Commits" and "review"
- [ ] `exists: .github/workflows/*.yml`; workflow triggers on push AND pull_request; lint job present 🔴
- [ ] `passes:` lint command; `passes:` format check
- [ ] One red CI run and one green CI run observed (the net works) 🔴
- [ ] `exists: .env.example`; `contains: .gitignore ".env"`; `absent: "password=|api_key|secret=" in tracked files` 🔴
- [ ] `git log` of main shows only PR/merge commits since protection

## Evidence

```
.github/workflows/ci.yml     CONTRIBUTING.md    lint/format configs
Link to first merged PR      CI run URLs (one red, one green)
```

## Failure Conditions

- Workflow exists but is disabled, or lint/test steps are skipped/`|| true`
- Anyone can push to main
- Secrets found anywhere in tracked files or history
- "First PR" was merged without review or with failing CI

## Completion Criteria

- [ ] All objectives verified; all 🔴 pass
- [ ] Full flow exercised once end-to-end (PR link recorded here)
- [ ] State + dashboard updated
