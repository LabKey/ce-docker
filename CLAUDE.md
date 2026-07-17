# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A minimal Docker Compose setup for running LabKey Community Edition locally. Two services: `community` (LabKey app on Tomcat) and `pg-community` (PostgreSQL — 18 by default, override with `PG_VERSION`).

## Common Commands

```bash
# Start
docker compose up community --detach

# Stop
docker compose down

# Stop and discard all volumes
docker compose down -v --remove-orphans

# Override image version at launch
COMPOSE_IMAGE="labkeyteamcity/labkey-community:26.3.0" docker compose up community --detach
```

Access at `https://localhost:8443` after startup (self-signed cert — expect browser warning).

## Configuration

All configuration is done via environment variables. `docker-compose.yml` uses `${VAR:-default}` expansion throughout — set variables in the shell or a `.env` file to override defaults.

Key overridable variables:
- `COMPOSE_IMAGE` — LabKey image tag (default: `labkeyteamcity/labkey-community:26.3.0`)
- `HOST_PORT` — host HTTPS port (default: `8443`)
- `PG_PORT` — host PostgreSQL port (default: `54321`)
- `PG_VERSION` — PostgreSQL major version / `pg-community` image tag (default: `18`); also the data-dir suffix
- `POSTGRES_PASSWORD` — shared between both services
- `MAX_JVM_RAM_PERCENT` — JVM heap ceiling (default: `75.0`)
- `LABKEY_CREATE_INITIAL_USER` / `LABKEY_CREATE_INITIAL_USER_APIKEY` — skip setup wizard if set
- SMTP variables (`SMTP_HOST`, `SMTP_PORT`, etc.) — email is disabled by default

## Persistent Data (./mounts/)

`mounts/` is gitignored. Do not delete these without understanding the consequences:
- `mounts/files/` — LabKey file storage
- `mounts/logs/` — server logs
- `mounts/pgdata/` — PostgreSQL data files (subdirectory keyed by `$IDENT` and `$PG_VERSION`; `IDENT` isolates pgdata across multiple concurrently-running instances, mirroring `LabKey/Dockerfile` — it does not affect which image is used, that's `COMPOSE_IMAGE`)
- `mounts/modules/` — custom/external LabKey modules

## Upgrading LabKey Version

Edit the `image:` line in `docker-compose.yml` (or set `COMPOSE_IMAGE`). Only tagged versions are published to Docker Hub; there is no `latest` tag.

## Branching and PR Workflow

The `.github/workflows/workflow.yml` runs `LabKey/gitHubActions/validate-pr@develop` on all PRs. It only executes for PRs from the `LabKey` org. Feature branches follow the pattern `fb_<version>_<description>` based on recent history.

## Commit and PR Body Formatting

Applies to every commit body and every PR body, without exception. Do not hard-wrap. Write each paragraph and each bullet as a single physical line, no matter how long. Separate paragraphs with one blank line. This applies to text passed via `-m`, `--body`, here-docs, `gh pr edit`, GitHub MCP tools — every channel that produces commit or PR body text.

**Why:** GitHub renders commit and PR bodies as GFM with hard-line-break enabled. Every mid-paragraph `\n` becomes a visible `<br>` in the rendered output, producing ragged, broken-looking text. Soft-wrap is the renderer's job, not yours.

**Self-check before invoking `git commit`, `gh pr create`, or `gh pr edit`:** look at the body text you are about to pass. If any paragraph spans more than one line in your tool call, that is a bug — collapse it to a single line first. Long lines are correct. Wrapped lines are wrong.

## Commit Messages

Subject: short imperative (≈70 chars). Body: follow the formatting rule above — one physical line per paragraph, blank lines between paragraphs.

## Pull Request Format

If the repo has a `pull_request_template.md` (typically under `.github/`), follow it. Otherwise, include sections for: **Rationale** (why the change is needed), **Related Pull Requests**, and **Changes** (notable items). Keep descriptions brief. Follow the formatting rule above — one physical line per paragraph and per bullet.

Before opening a PR, always draft the title and description and confirm them with the user. Do not run `gh pr create` until the user approves.
