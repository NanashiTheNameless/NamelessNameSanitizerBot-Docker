# Claude usage notes for this repo

This repository is a Docker runner for the NamelessNameSanitizerBot image. Use Claude to make focused documentation and configuration updates. There’s no app source code here.

## What Claude can help with

- Edit `.env.example` defaults and comments
- Improve `README.md` sections (Quick start, Troubleshooting)
- Add operational guides (backups, upgrades, restoring the DB volume)
- Review `docker-compose.yml` for clarity and portability

## Constraints

- Don’t introduce secrets into the repo. Keep real values in `.env` only.
- Avoid changing the image references unless asked.
- Keep diffs minimal - only touch lines you need to change.

## Project facts Claude should remember

- Services: `bot` + `db` (`postgres:18`)
- Defaults currently: `COOLDOWN_SECONDS=30`, `SWEEP_INTERVAL_SEC=60`
- Healthcheck exists for Postgres
- Volume named `db` stores Postgres data

## Quick validation commands

```bash
# Validate compose
docker compose config

# Start or restart
docker compose up -d

# Tail recent logs
docker compose logs -n 100 bot
```

See `AGENTS.md` for general agent guidelines and `COPILOT.md` for VS Code-specific tips.
