# Phase 09 — Operations

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** keep the live product healthy: it restarts itself, its data is backed up, and there's a manual anyone can follow at 2am.

---

## Objectives

- [ ] Health endpoint (`/health` or `/healthz`) reporting app + dependency status
- [ ] Restart policies / process supervision: crashed containers restart automatically; graceful shutdown handled
- [ ] Resource limits set on containers (memory/CPU) so one runaway service can't kill the host
- [ ] Automated backups scheduled for ALL persistent data; retention defined
- [ ] Restore rehearsed: real backup restored into a scratch environment, data verified
- [ ] Backup/restore cadence checked against the RTO/RPO targets set in `docs/reliability.md` (Phase 06) — if backups run less often than the RPO allows, that's a gap, not a technicality
- [ ] Failover thinking recorded: single points of failure listed; acceptable-downtime decision by owner
- [ ] Runbook `docs/operations.md`: check health, read logs, restart, roll back, restore backup, escalation contacts

## Verification — the agent MUST verify

- [ ] `passes:` health endpoint returns 200 + status body
- [ ] Kill test: stop a container, it comes back automatically 🔴
- [ ] Backup job exists AND a recent backup artifact is present
- [ ] Restore rehearsal recorded with date + result 🔴
- [ ] Backup cadence vs RPO cross-check recorded 🔴
- [ ] `exists: docs/operations.md` covering all runbook topics; a second person (or fresh agent session) followed it successfully 🔴

## Evidence

```
curl /health     restart policy in compose/orchestrator
backup job config + artifact listing     restore drill note
docs/operations.md
```

## Failure Conditions

- Backups scheduled but never restored ("a backup that was never restored is a hope, not a backup")
- Health endpoint returns 200 while a dependency is down
- Runbook exists but references commands that don't work
- Backup frequency doesn't actually meet the stated RPO

## Completion Criteria

- [ ] All objectives verified; all 🔴 pass
- [ ] State + dashboard updated
