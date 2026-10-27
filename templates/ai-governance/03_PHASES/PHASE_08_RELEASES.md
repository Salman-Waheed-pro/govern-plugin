# Phase 08 — Release & Production Readiness

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** turn "works on staging" into "version 1.0.0 live for real users" — with version numbers that mean something, human-readable release notes, and a launch checklist that catches the boring mistakes.

⚠️ **Production launch additionally requires Phase 11 (Security) gates to be green.** The final production deploy is blocked until it passes.

---

## Objectives

- [ ] Semantic Versioning adopted: MAJOR breaking / MINOR feature / PATCH fix (conventional commits can compute this)
- [ ] First annotated tag `v1.0.0` created and pushed
- [ ] `CHANGELOG.md` with entries per version, newest first
- [ ] Tags trigger the release pipeline (tag → build → publish `vX.Y.Z` image → deploy)
- [ ] Feature-flag mechanism available for risky features; deployment ≠ release
- [ ] Environments separated: dev / staging / production, each with own config + secrets; production most restricted
- [ ] Production provisioned; deploy config pins the exact release tag (never `latest`)
- [ ] Production Readiness Checklist completed → `docs/release-checklist-v1.md`: DNS+TLS auto-renew, backups scheduled+tested, error pages, privacy/terms if personal data, support contact, load sanity check (expected launch traffic ×3)
- [ ] Production deploy behind manual approval; post-deploy smoke tests on production
- [ ] Release notes published, readable by non-developers
- [ ] Production rollback rehearsed once (to previous tag)

## Objectives — staged rollout, kill switches & compliance gate

- [ ] Canary or staged rollout used for the production deploy where feasible (e.g. 10% of traffic/instances first, then 100%) — or explicitly recorded as N/A for small-scale launches with a reason
- [ ] The riskiest new feature in this release is confirmed reachable behind the Phase 06 kill switch in production (not just staging)
- [ ] **License compliance check:** every dependency's license is compatible with how this product is distributed (e.g. no copyleft-in-proprietary-SaaS surprise) → recorded in `docs/license-audit.md`
- [ ] Compliance reporting duty checked: if the product is subject to any regulatory reporting (data breach notification, accessibility, industry-specific), the obligation and owner are named

## Verification — the agent MUST verify

- [ ] `git tag -n` shows annotated `v*` tag; `exists: CHANGELOG.md` with matching section
- [ ] `ci:` tag trigger configured; observed release run
- [ ] Running production image tag == git tag 🔴
- [ ] `exists: docs/release-checklist-v1.md` fully checked with owner signature line 🔴
- [ ] Phase 11 security gates verified green before the production deploy 🔴
- [ ] Rollback rehearsal recorded (date, duration) 🔴
- [ ] `exists: docs/license-audit.md` with no unresolved blocking incompatibilities 🔴
- [ ] Canary/staged rollout config present, or N/A recorded with an explicit reason

## Evidence

```
git tag -n     CHANGELOG.md     release pipeline run URL
docs/release-checklist-v1.md (signed)     docs/license-audit.md
production URL     smoke results     kill-switch-in-production note
```

## Failure Conditions

- Anything hand-patched directly on the server
- Checklist items silently unchecked, or signature missing
- Version deployed differs from version tested on staging
- Security phase not green
- A copyleft dependency shipped in a closed-source product with no legal review

## Completion Criteria

- [ ] All objectives verified; all 🔴 pass
- [ ] State + dashboard updated; owner notified of go-live in plain English
