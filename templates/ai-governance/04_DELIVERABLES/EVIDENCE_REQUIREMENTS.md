# EVIDENCE REQUIREMENTS

What counts as proof. If evidence doesn't meet this bar, the item stays unchecked.

## The bar

1. **Existence is not enough.** A file must have real content serving its purpose. A 3-line `architecture.md` stub, an empty test that always passes, a checklist template with no answers — all FAIL.
2. **Recency matters.** Evidence referencing deleted files or long-gone CI runs is stale; re-verify.
3. **Commands beat claims.** Where a command can prove it (`docker compose up`, test suite, `curl /health`), run the command; record the exact command and outcome.
4. **Negative evidence needs a real search.** "No secrets in repo" requires the grep/scan actually run, with patterns and scope recorded.
5. **Human sign-offs are named and dated** inside the deliverable file itself, not in chat.

## Evidence format (inline in phase files)

```
✅ YYYY-MM-DD — evidence: docs/charter.md ("Approved by: <name>, <date>")
✅ YYYY-MM-DD — evidence: `docker compose up` → 3/3 services healthy
✅ YYYY-MM-DD — evidence: gitleaks full-history scan → 0 findings
```

## Requirement example (the pattern every deliverable follows)

**Requirement:** Docker installed
**Evidence required:** `docker --version` output
**Verification:** agent runs `docker version`
**Expected:** client + server respond, version within support window
**Status:** ✅ / ❌ with the actual output line

## Verification vocabulary

See `07_AI_AGENT_RULES.md` — `exists:` / `contains:` / `absent:` / `passes:` / `ci:` / `human:`. Adapt concrete commands to the project's stack and OS; record what was actually run.
