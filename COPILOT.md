# Copilot usage notes for this repo (VS Code)

This repository contains only a Docker Compose runner and docs for NamelessNameSanitizerBot. Use Copilot to streamline edits and small ops tasks.

## Recommended workflow

- Open the repo folder in VS Code
- Use the SCM view to review diffs Copilot proposes
- Keep edits targeted: `.env.example`, `README.md`, `docker-compose.yml`
- When adding docs, prefer short sections with copy-pasteable bash commands

## Typical Copilot tasks

- Update environment variable defaults and comments
- Add troubleshooting or operations sections
- Propose safe compose tweaks (e.g., bind-mount `./db`)
- Create lightweight CI or scripts if desired

## Project specifics to keep in mind

- No app source code here; don’t introduce files implying a build
- Services: `bot` (published image) and `db` (postgres:18)
- Defaults: `COOLDOWN_SECONDS=30`, `SWEEP_INTERVAL_SEC=60`
- Healthcheck ensures `db` is ready before bot starts

## Handy commands

```bash
# Bring up services
docker compose up -d

# Pull latest image
docker compose pull && docker compose up -d

# Show logs
docker compose logs -n 100 bot
```

See also: `AGENTS.md` for general guidelines and `CLAUDE.md` for provider tips.
