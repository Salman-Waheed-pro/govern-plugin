# Runbook — GitHub Actions (CI/CD)

## Inspect & operate

```bash
gh run list --limit 10               # recent runs
gh run view <id> --log-failed        # why it failed
gh run rerun <id>                    # re-run
gh workflow list                     # workflows + state (check none disabled!)
gh secret list                       # secret NAMES (values never visible)
gh secret set NAME                   # add/rotate a secret
act                                  # run workflows locally (if installed)
```

## Reference pipeline skeleton

```yaml
name: ci
on: { push: { branches: [main] }, pull_request: {} }
jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install     # setup-node / setup-python + cached deps
        run: <install>
      - run: <lint>
      - run: <format-check>
      - run: <test with coverage>
  build:
    needs: quality
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: docker build -t ghcr.io/<org>/<app>:${{ github.sha }} .
      - run: docker push ghcr.io/<org>/<app>:${{ github.sha }}
  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy      # ssh + compose pull/up, or platform action
        run: <deploy command using secrets>
      - name: Smoke test
        run: curl -fsS https://staging.<domain>/health
```

## Rollback

```bash
# Redeploy previous good SHA/tag:
<deploy command> --image ghcr.io/<org>/<app>:<previous-tag>
# or: git tag → re-run release workflow on previous tag
```

## Rules

- Secrets ONLY via `${{ secrets.NAME }}` — never inline, never in committed env files.
- Tags `v*` trigger the release pipeline (Phase 07).
- Security scan jobs (Phase 10) must not be `continue-on-error` for criticals.
