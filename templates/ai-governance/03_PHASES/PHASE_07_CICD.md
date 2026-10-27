# Phase 07 — CI/CD Pipeline

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** automate the road from "code merged" to "running on a server". Humans stop copying files to servers; the pipeline builds, tests, packages, and deploys the same way every time.

**Target pipeline:** Push/PR → Lint → Unit tests → Integration tests → Build → Docker image → Push image to registry (tagged) → Deploy STAGING → Smoke tests → [manual approval] → PRODUCTION (activates Phase 08).

---

## Objectives

- [ ] CI builds the Docker image on every merge to main
- [ ] Images pushed to a registry (GHCR/Docker Hub/cloud) tagged with git SHA + branch — never only `latest`
- [ ] ALL pipeline credentials in the CI secret store; zero secrets in workflow files
- [ ] Staging environment provisioned, mirroring production shape → recorded in `docs/environments.md`
- [ ] Auto-deploy to staging on merge to main (no manual file copying)
- [ ] Post-deploy smoke tests: health endpoint + one core flow; failure marks deploy red
- [ ] Rollback procedure written AND rehearsed: previous image tag restored in ≤5 minutes
- [ ] Pipeline ≤ ~10 min via dependency + layer caching
- [ ] `docs/pipeline.md`: stages, triggers, secret names (names only), re-run + rollback steps

## Objectives — build & supply-chain security

- [ ] Build scripts reviewed: no `curl | bash` from untrusted sources; no unpinned third-party scripts executed during CI
- [ ] Third-party CI actions/plugins pinned to a commit SHA, not a floating tag (`uses: org/action@<sha>`, not `@v1`) — floating tags are a supply-chain attack vector
- [ ] CI logs checked to confirm secrets are masked/redacted, not printed via debug output or `echo`
- [ ] Build artifacts (images) are content-addressed/digest-pinned in deploy configs so what was tested is provably what gets deployed

## Verification — the agent MUST verify

- [ ] Workflow contains build/push/deploy stages; triggers correct
- [ ] Registry shows SHA-tagged images
- [ ] `absent:` credentials in `.github/workflows/**` 🔴
- [ ] Third-party actions pinned to commit SHA, not tag 🔴
- [ ] Observed run: merge → staging updated automatically → smoke test green 🔴 (record run URL)
- [ ] Rollback rehearsal recorded: date, who, duration 🔴
- [ ] Deliberately trigger a debug/log step and confirm no secret value appears in the run log
- [ ] `exists: docs/pipeline.md, docs/environments.md`

## Evidence

```
.github/workflows/*.yml     registry image list
staging URL     pipeline run URL     rollback rehearsal note
```

## Failure Conditions

- Deploy step requires manual server login / file copy
- Smoke tests missing or not blocking
- Rollback documented but never executed
- Secrets inline in YAML or committed env files
- Third-party actions referenced by mutable tag (`@main`, `@v1`) instead of pinned SHA

## Completion Criteria

- [ ] All objectives verified; all 🔴 pass
- [ ] State + dashboard updated
