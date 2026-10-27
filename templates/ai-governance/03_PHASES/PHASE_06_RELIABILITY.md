# Phase 06 — Reliability & Data Integrity

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** code that passes every test can still corrupt data, duplicate a payment, or fall over under two things happening at once. This phase hardens the parts of the system that never show up in a demo but decide whether it survives real, messy, concurrent, partially-failing traffic. This is the single biggest gap in most AI-generated codebases — models write code that works on the happy path and silently skip the failure paths.

**Entry criteria:** Phase 05 in progress or complete.

---

## Objectives — data integrity

- [ ] Every multi-step operation that changes data has a defined transaction boundary — all steps succeed together or none do; no operation leaves half-written data on failure
- [ ] Idempotency keys (or equivalent: unique constraints, dedup tables) implemented for any retryable external effect — payments, webhooks, emails, order creation
- [ ] Database migrations reviewed for safety on a populated table: no long-held locks, backward-compatible during rollout (old and new code can both run against the schema mid-deploy), and a documented rollback or forward-fix path
- [ ] Destructive operations (bulk delete, mass update, drop) require a confirmation step, soft-delete, or dry-run mode — never a single irreversible action with no undo window

## Objectives — failure handling

- [ ] Errors classified as retryable vs non-retryable; retries are bounded with backoff — no unbounded retry loops anywhere in the codebase
- [ ] Failures fail loudly: logged and (if user-facing) surfaced clearly — nothing is silently swallowed in a bare `except: pass` / empty `catch` block
- [ ] Background jobs/queues: failed messages route to a dead-letter queue (or equivalent) instead of being lost or retried forever; queue consumers are themselves idempotent
- [ ] Orphaned-resource cleanup exists for temp files, abandoned uploads, expired sessions/records

## Objectives — concurrency & shared state

- [ ] Race conditions reviewed on every code path where two requests could touch the same record simultaneously; the chosen mechanism (DB row locks, optimistic concurrency/version column, application-level locking) is documented, not assumed
- [ ] TOCTOU (time-of-check-to-time-of-use) gaps checked on security-sensitive checks (e.g. checking a permission then acting on it later) — check and act must be atomic or re-verified

## Objectives — resource safety & scale-adjacent reliability

- [ ] Every outbound call (HTTP, DB, queue) has an explicit timeout — no call can hang a request indefinitely
- [ ] Connection pooling configured with a bounded pool size; no unbounded connection growth under load
- [ ] No unbounded in-memory caches/queues (every cache has a max size or TTL) — this is how "small" memory leaks become production outages
- [ ] API responses are scoped to what the client needs (no chronic over-fetching); list endpoints are paginated and reject unbounded page sizes
- [ ] Request payload size limits enforced (protects against resource-exhaustion / trivial DoS via huge uploads or bodies)

## Objectives — resilience & recovery planning

- [ ] Single points of failure identified (cross-reference the Phase 02 scalability note); for each, an owner-approved decision recorded: fixed now, or accepted risk with a reason
- [ ] Cache invalidation strategy documented; cache cannot be poisoned by unvalidated input (e.g. caching a response keyed on attacker-controlled headers)
- [ ] Feature flags have a named owner and an expiry/review date — no permanent flag sprawl; at least one flag has been proven to work as a kill switch (disables a feature in production with no deploy)
- [ ] Disaster recovery targets defined: Recovery Time Objective (how long can it be down) and Recovery Point Objective (how much data can be lost) — and the Phase 09 backup/restore cadence is checked to actually meet them

## Verification — the agent MUST verify

- [ ] Transaction boundaries reviewed for the top 3 state-changing flows; findings in `docs/reliability.md` 🔴
- [ ] Idempotency test (from Phase 05) passes for the core operation 🔴
- [ ] `absent:` unbounded retry loops (no max-attempts/backoff) in code 🔴
- [ ] Migration dry-run executed against production-shaped data; rollback path documented
- [ ] Dead-letter queue configured if async queues are used — or N/A recorded with reason
- [ ] Kill-switch demonstrated: a flagged feature disabled without a deploy, effect observed and recorded
- [ ] Every outbound call site sampled for a timeout; gaps listed and fixed or accepted
- [ ] RTO/RPO stated and cross-checked against Phase 09's actual backup frequency 🔴
- [ ] `exists: docs/reliability.md` covering all sections above with owner review

## Evidence

```
docs/reliability.md            migration dry-run log
idempotency test result        kill-switch demo note
RTO/RPO vs backup cadence cross-check
```

## Failure Conditions — do NOT mark complete if

- "We'll add idempotency if duplicate charges become a problem" for a payment/order path
- A migration ran directly on production with no dry-run or rollback plan
- Retries with no cap/backoff found anywhere in the codebase
- Feature flags exist with no owner or expiry ("flag graveyard")
- RTO/RPO stated but backups run less often than RPO requires

## Completion Criteria

- [ ] All objectives verified; all 🔴 pass
- [ ] State + dashboard updated
