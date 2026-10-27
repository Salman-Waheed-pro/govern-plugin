# Phase 04 — Docker & Containerization

Status:
- [ ] Not Started
- [ ] In Progress
- [ ] Completed

**Plain English:** package the app so it runs identically everywhere — laptop, teammate's PC, server. Kills "works on my machine".

---

## Objectives

- [ ] Docker installed and daemon running
- [ ] `Dockerfile`: official **pinned** base image (never `latest`), WORKDIR, COPY, RUN, EXPOSE, CMD; multi-stage build if it shrinks the image; `USER` non-root
- [ ] `.dockerignore` excluding node_modules/venv, .git, `.env`, build junk
- [ ] Project containerized: image builds, container serves the app
- [ ] `docker-compose.yml` (or `compose.yaml`): full local stack (app + db + services), named volumes for persistent data, healthchecks
- [ ] ALL configuration externalized to environment variables; each documented in `.env.example`; distinguish build-time ARG vs runtime ENV
- [ ] Persistence proven: create data → destroy containers → recreate → data intact
- [ ] Container commands documented in README + `05_RUNBOOKS/Docker.md`

## Verification — the agent MUST verify

- [ ] `exists: Dockerfile` — `contains: "USER"`, base image pinned, `absent: secrets`
- [ ] `exists: .dockerignore` containing `.env`
- [ ] `exists: docker-compose.yml|compose.yaml`; `passes: docker compose config`
- [ ] `passes: docker build .`
- [ ] `passes: docker compose up -d` then `docker compose ps` shows all services healthy 🔴
- [ ] Volumes persist data across `docker compose down && up` (run the test) 🔴
- [ ] `absent:` hardcoded hosts/ports/credentials in image and compose file 🔴 (check `docker history` for leaked secrets)

## Evidence

```
Dockerfile   .dockerignore   docker-compose.yml
docker compose up   docker compose ps   docker compose logs
Persistence test result recorded here with date
```

## Failure Conditions — DO NOT mark complete if

- Image builds but container crashes or app unreachable
- Data lost after container recreation
- Secrets in Dockerfile, compose, or image layers
- Base image `latest`, or container runs as root without a recorded reason

## Completion Criteria

- [ ] All objectives verified; all 🔴 pass
- [ ] One command (`docker compose up`) starts the whole stack from a clean checkout
- [ ] State + dashboard updated
