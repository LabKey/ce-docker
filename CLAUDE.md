# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A minimal Docker Compose setup for running LabKey Community Edition locally. Two services: `community` (LabKey app on Tomcat) and `pg-community` (PostgreSQL 17).

## Common Commands

```bash
# Start
docker compose up community --detach

# Stop
docker compose down

# Stop and discard all volumes
docker compose down -v --remove-orphans

# Override image version at launch
export IDENT="labkeyteamcity/labkey-community:26.3.0" && docker compose up community --detach
```

Access at `https://localhost:8443` after startup (self-signed cert — expect browser warning).

## Configuration

All configuration is done via environment variables. `docker-compose.yml` uses `${VAR:-default}` expansion throughout — set variables in the shell or a `.env` file to override defaults.

Key overridable variables:
- `COMPOSE_IMAGE` — LabKey image tag (default: `labkeyteamcity/labkey-community:26.3.0`)
- `HOST_PORT` — host HTTPS port (default: `8443`)
- `PG_PORT` — host PostgreSQL port (default: `54321`)
- `POSTGRES_PASSWORD` — shared between both services
- `MAX_JVM_RAM_PERCENT` — JVM heap ceiling (default: `75.0`)
- `LABKEY_CREATE_INITIAL_USER` / `LABKEY_CREATE_INITIAL_USER_APIKEY` — skip setup wizard if set
- SMTP variables (`SMTP_HOST`, `SMTP_PORT`, etc.) — email is disabled by default

## Persistent Data (./mounts/)

`mounts/` is gitignored. Do not delete these without understanding the consequences:
- `mounts/files/` — LabKey file storage
- `mounts/logs/` — server logs
- `mounts/pgdata/` — PostgreSQL data files (subdirectory keyed by `$IDENT`)
- `mounts/modules/` — custom/external LabKey modules

## Upgrading LabKey Version

Edit the `image:` line in `docker-compose.yml` (or set `COMPOSE_IMAGE`). Only tagged versions are published to Docker Hub; there is no `latest` tag.

## Branching and PR Workflow

The `.github/workflows/workflow.yml` runs `LabKey/gitHubActions/validate-pr@develop` on all PRs. It only executes for PRs from the `LabKey` org. Feature branches follow the pattern `fb_<version>_<description>` based on recent history.
