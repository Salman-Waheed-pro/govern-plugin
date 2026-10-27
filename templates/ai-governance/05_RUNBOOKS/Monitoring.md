# Runbook — Monitoring & Observability

## Quick health check (first move in any incident)

```bash
curl -fsS https://<domain>/health          # app + dependencies
docker compose ps                          # or kubectl get pods
docker compose logs --since 15m <svc>      # recent logs
df -h && free -m                           # disk/memory on host
```

## Where things live (fill in Phase 09)

```
Dashboard:      <URL>
Error tracker:  <URL>            # Sentry/GlitchTip project
Logs:           <where/how to search>
Alert channel:  <channel>
Uptime monitor: <URL>
```

## Alert rules (minimum set)

| Alert | Condition | Route |
|---|---|---|
| Site down | health check fails 2× in a row | <channel> immediately |
| Error spike | error rate > Nx baseline over 5 min | <channel> |
| Disk | > 85% used | <channel> |
| Cert expiry | < 14 days | <channel> |

## Drills (gates)

```
Fire drill (Phase 09): break staging → alert fires → human acks → record date
Restore drill (monthly): restore backup to scratch env → verify → record in PHASE_12 loop log
```

## Reading the three pillars

- **Logs** answer "what happened" — search by request id.
- **Metrics** answer "how much/how fast" — check error rate & p95 latency FIRST.
- **Traces** answer "where did the time go" — for cross-service slowness.
