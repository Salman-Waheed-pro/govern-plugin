# Phase 05 — Testing

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** build the safety net that finds failures before users do. "If it's not tested, it's broken — you just don't know it yet."

---

## Strategy — the testing pyramid

Many fast **unit tests** (single functions/classes, Arrange–Act–Assert, deterministic) → fewer **integration tests** (components together: service + real database, preferably in containers) → few **end-to-end tests** (whole user journeys). Mock external paid/slow services (payments, email) — but don't mock so much the tests prove nothing.

## Objectives — functional coverage

- [ ] Test runner configured; whole suite runs with ONE command (see `05_RUNBOOKS/Testing.md`)
- [ ] Unit tests for core business logic — every module from `docs/architecture.md` has tests for its public interface
- [ ] Integration tests for critical paths (API + database at minimum), using containerized dependencies where possible
- [ ] API tests: every endpoint tested for success, validation failure, and auth failure
- [ ] Frontend component tests for key user interactions (behavior, not implementation details) — or N/A recorded with reason
- [ ] ≥1 end-to-end test covering the core user journey (the MVP assumption from Phase 02)
- [ ] Coverage threshold agreed with owner (recommended start: 70% on core logic) and enforced in config
- [ ] Tests wired into CI: every PR runs the suite; failures block merge

## Objectives — reliability & hygiene testing (closes the gap between "passes tests" and "survives production")

- [ ] **Test isolation:** no test mutates shared/global state that leaks into another test; suite gives the same result run in any order and in parallel
- [ ] **Concurrency tests:** for every code path where two requests could touch the same record at once (e.g. double-submit, parallel updates), a test proves the outcome is correct, not just "usually correct"
- [ ] **Idempotency tests:** for the core state-changing operation (payment, order, signup — whatever the product's money path is), repeating the identical request (retry, double-click, duplicate webhook delivery) produces ONE effect, not many
- [ ] **No production data in tests, ever.** Test/staging fixtures are synthetic or genuinely anonymized (not just renamed fields) — never a copy of real customer data
- [ ] **Lightweight chaos check:** kill a dependency (DB connection, cache, a downstream call) mid-request in a test/staging run and confirm the system degrades gracefully (clear error, no data corruption) instead of crashing silently or half-writing data
- [ ] A full-scale load test (≥3–10× expected launch traffic) is scheduled before the Phase 08 production go-live — this phase only needs the harness to exist and run once against staging

## Verification — the agent MUST verify

- [ ] `passes:` full suite via the single command
- [ ] Coverage report generated; number ≥ threshold; threshold enforced in config 🔴
- [ ] CI workflow runs tests on PR; a deliberately-broken test demonstrably blocks a PR 🔴 (record the PR link + date)
- [ ] Named e2e test covers the core journey 🔴
- [ ] Idempotency test exists and passes for the core state-changing operation 🔴
- [ ] `absent:` real customer data / PII in any file under test-fixtures or seed data directories 🔴
- [ ] Chaos check executed at least once; result recorded with date
- [ ] `exists: docs/testing.md`

## Evidence

```
test command + output      coverage report
CI run blocked by red test (URL)      docs/testing.md
idempotency test name + result        chaos-check note
```

## Failure Conditions

- Tests exist but are skipped/disabled in CI, or `continue-on-error` set
- Coverage counted on trivial files while business logic is untested
- E2E test exists but doesn't run in any pipeline
- Suite is flaky (different results on re-run) — fix before completing
- Test fixtures contain real user data "because it was convenient"
- "We'll test idempotency if duplicate charges become a problem" — for a money/state path this is not optional

## Completion Criteria

- [ ] All objectives verified; all 🔴 pass
- [ ] State + dashboard updated
