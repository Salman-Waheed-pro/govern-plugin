# Runbook — Docker

> Fill the `<project-specific>` lines during Phase 04. The agent may execute these.

## Daily

```bash
docker compose up -d            # start full stack
docker compose ps               # status + health
docker compose logs -f <svc>    # follow logs
docker compose exec <svc> sh    # shell into a service
docker compose down             # stop (volumes kept)
docker compose down -v          # stop AND DELETE DATA — ask owner first
```

## Build & debug

```bash
docker build -t <name>:dev .
docker compose config           # validate compose syntax
docker inspect <container>      # full config/state
docker stats                    # live resource usage
docker system df                # disk usage; prune with care: docker system prune
docker history <image>          # check layers (e.g. for leaked secrets)
```

## Persistence test (Phase 04 gate)

```bash
# create data in app → then:
docker compose down && docker compose up -d
# verify data still present; record result in PHASE_04
```

## Project-specific

```
Build:   <docker build command>
Run:     <docker compose up>
Migrate: <db migration command inside container>
Seed:    <seed command>
```
