# Runbook — Testing

> Fill the concrete commands in Phase 05. One command must run everything.

## Commands (project-specific)

```
All tests:        <npm test | pytest | go test ./...>
Unit only:        <cmd>
Integration:      <cmd>            # may need: docker compose up -d db
E2E:              <cmd>            # may need running app
Coverage:         <cmd>            # threshold enforced in config at: <file>
Single test:      <cmd -k / -t pattern>
Watch mode:       <cmd>
Lint + format:    <cmd>
```

## Conventions

- Structure: Arrange – Act – Assert; one behavior per test; name says the behavior (`rejects_expired_token`)
- Unit tests: no network, no real DB, deterministic, < 1s each
- Integration: real DB in a container; migrations applied in test setup
- Mock ONLY external paid/slow services; if everything is mocked the test proves nothing
- Flaky test = broken test: fix or quarantine with a board card, never ignore

## CI expectations (gates)

- Suite runs on every PR; red suite blocks merge (no `continue-on-error`, no skipped steps)
- Coverage threshold enforced by config, not promise
- E2E for the core journey runs at minimum before each release

## Troubleshooting

```
Works locally, fails in CI → env var missing? service container absent? timezone/locale?
DB tests fail → is the test DB migrated? parallel tests sharing state?
Flaky → time-dependence, ordering, shared fixtures, real network calls
```
