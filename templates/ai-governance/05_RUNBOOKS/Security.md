# Runbook — Security Tooling

> Concrete commands for the Phase 10 hunt and the permanent CI guards. Use whichever tools fit the stack; record what was run.

## Secrets

```bash
gitleaks detect --source . -v                    # working tree
gitleaks detect --source . --log-opts="--all"    # FULL history (Phase 10 gate)
trufflehog git file://. --only-verified          # alternative
# leaked? → rotate the credential FIRST, then clean history (BFG/filter-repo), record in security-review
```

## Dependencies

```bash
npm audit --audit-level=high        # Node
pip-audit                           # Python
osv-scanner -r .                    # any ecosystem
# + enable Dependabot/Renovate in repo settings
```

## Static analysis (SAST)

```bash
semgrep scan --config auto .        # multi-language
bandit -r src/                      # Python
# CodeQL: enable in GitHub → Security → Code scanning
```

## Container images

```bash
trivy image <image>:<tag> --severity HIGH,CRITICAL
grype <image>:<tag>
trivy config .                      # misconfig in Dockerfile/compose/k8s
```

## Manual hunt greps (starting points — confirm every hit by reading code)

```bash
# SQL injection surface
grep -rnE '"(SELECT|INSERT|UPDATE|DELETE).*"\s*\+|f".*(SELECT|INSERT|UPDATE|DELETE)|\.format\(.*(SELECT|WHERE)' src/
# command injection
grep -rnE 'child_process\.exec\(|os\.system\(|shell=True|eval\(|new Function\(' src/
# XSS sinks
grep -rnE 'innerHTML|dangerouslySetInnerHTML|v-html|document\.write|\.html\(|\|\s*safe' src/
# hardcoded secrets
grep -rnEi '(api[_-]?key|secret|password|token)\s*[:=]\s*["'"'"'][^"'"'"']{8,}' --exclude-dir=node_modules .
# weak crypto / TLS off
grep -rnE 'md5\(|sha1\(|verify\s*=\s*False|rejectUnauthorized:\s*false' src/
# unsafe deserialization
grep -rnE 'pickle\.loads|yaml\.load\((?!.*SafeLoader)|unserialize\(' src/
```

## Headers & endpoint checks

```bash
curl -sI https://<staging>/ | grep -iE 'strict-transport|content-security|x-content-type|x-frame'
# login rate limit: script 20 rapid failed logins → expect 429/backoff
```

## Triage rule

Critical/high → fix now. Medium → board card with deadline. Accepted risk → written owner sign-off in `docs/security-review.md`. Never "warn-only" for criticals in CI.
