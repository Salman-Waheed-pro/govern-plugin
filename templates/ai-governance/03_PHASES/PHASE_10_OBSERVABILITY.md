# Phase 10 — Observability

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** be the FIRST to know when something breaks — before users tell you. Three pillars: logs (what happened), metrics (how much/how fast), traces (where time went).

---

## Objectives

- [ ] Structured logging (JSON/key-value): level, timestamp, request id; **no passwords, tokens, or personal data in logs**
- [ ] Logs centralized/retained beyond container lifetime and searchable
- [ ] Error tracking integrated (Sentry/GlitchTip/equivalent): unhandled errors reach a dashboard with stack traces
- [ ] Metrics collected: request rate, error rate, latency (p50/p95), CPU/memory, disk
- [ ] Dashboard showing service health at a glance; URL in `docs/operations.md`
- [ ] Alert rules: site down, error-rate spike, disk nearly full — routed to a channel humans actually watch
- [ ] **Anomaly/abuse alerting:** a rule exists for sudden, abnormal spikes (one account/IP generating far more traffic than normal, sudden signup surge) — an early signal for both abuse and unplanned scale events
- [ ] Fire drill: deliberately break staging (or brief prod window) → alert fires → human acknowledges
- [ ] (If microservices) request tracing/correlation ids across services

## Verification — the agent MUST verify

- [ ] Log samples are structured; `absent:` secrets/PII in log output 🔴
- [ ] Test error appears in the error tracker (trigger one deliberately)
- [ ] Dashboard exists; URL recorded
- [ ] Fire drill recorded: date, alert screenshot/note, who acknowledged 🔴
- [ ] Anomaly/abuse alert rule exists and its threshold is documented
- [ ] Logs survive `docker compose down && up`

## Evidence

```
log excerpt     error-tracker event URL     dashboard URL
alert rule config     fire-drill note in docs/operations.md
```

## Failure Conditions

- Monitoring exists but nobody receives alerts ("decoration, not monitoring")
- Sensitive data found in logs
- Error tracker configured but DSN empty / disabled in production config
- No alert would ever fire on an abuse/traffic-spike scenario

## Completion Criteria

- [ ] All objectives verified; all 🔴 pass
- [ ] State + dashboard updated
