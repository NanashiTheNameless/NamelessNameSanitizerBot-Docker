# Using AI Agents with NamelessNameSanitizerBot-Docker

This repository is a minimal Docker runner for the NamelessNameSanitizerBot image. There is no application source code here. AI assistants can still help you automate environment setup, documentation updates, CI, and operations.

Quick links:

- Main bot source and full docs: <https://github.com/NanashiTheNameless/NamelessNameSanitizerBot>
- This repo: Docker Compose runner + `.env.example` and `docker-compose.yml`

## Scope of this repo

- Docker Compose stack to run:
  - `bot`: `nanashithenameless/namelessnamesanitizerbot:latest`
  - `db`: `postgres:18` with a healthcheck and volume
- Configuration via `.env` (copy from `.env.example`)
- No Python/TypeScript application source exists here

## Typical tasks for agents

- Customize `.env.example` defaults and docs
- Update `README.md` sections or add usage guides
- Create operational docs (backup/restore, upgrades, troubleshooting)
- Adjust Compose options (volumes, bind mounts, healthchecks)
- Add minimal CI to lint YAML or validate Compose

## Guardrails and safety

- Do not hardcode secrets. Keep `DISCORD_TOKEN` only in local `.env`.
- Don’t modify the published image name unless instructed.
- Keep changes minimal and focused; avoid reformatting unrelated lines.
- Do not modify formatting of README.md unless explicitly asked, It appears to have errors, this is intentional.
- Verify changes: `docker compose config` to validate, and restart if needed.

## Environment variables (key ones)

- `DISCORD_TOKEN` – required
- `APPLICATION_ID` – used for invite URL
- Policy: `CHECK_LENGTH`, `MIN_NICK_LENGTH`, `MAX_NICK_LENGTH`, `PRESERVE_SPACES`, `SANITIZE_EMOJI`, `ENFORCE_BOTS`
- Cooldowns: `COOLDOWN_SECONDS` (default 30), `COOLDOWN_TTL_SEC`
- Background sweep: `SWEEP_INTERVAL_SEC` (default 60), `SWEEP_BATCH`
- `DATABASE_URL` – stays `postgresql://bot:bot@db:5432/bot` by default

## Acceptance criteria for edits

- Docs should be concise, accurate, and reflect the current Compose file
- Any new commands must be copy-paste friendly for bash
- If you change defaults, update `.env.example` and mention in README when relevant

## Validation checklist

- Run: `docker compose config` (valid YAML?)
- Start: `docker compose up -d` (services healthy?)
- Logs: `docker compose logs -n 50 bot` (app started?)
- Invite URL: based on `APPLICATION_ID`

See provider-specific tips in `CLAUDE.md` and `COPILOT.md`.
