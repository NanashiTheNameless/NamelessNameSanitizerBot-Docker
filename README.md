# NamelessNameSanitizerBot — Minimal Docker Runner

This repository is a minimal, fast-to-run Docker setup for NamelessNameSanitizerBot. It contains only what you need to run the published Docker image with PostgreSQL via Docker Compose—no application source code.

If you’re looking for the bot’s source code, feature list, and full documentation, see:

- Full repo and docs: https://github.com/NanashiTheNameless/NamelessNameSanitizerBot
- Install to a server: https://namelessnamesanitizerbot.namelessnanashi.dev/install/

## Why this repo exists

- Minimal footprint: just `docker-compose.yml`, `.env.example`, and a couple of docs
- Zero local build: uses the published image `nanashithenameless/namelessnamesanitizerbot:latest`
- One command to start: bring up the bot and a PostgreSQL instance quickly

## Requirements

- Docker Engine and Docker Compose plugin
- Discord Bot token (with Bot scope; recommended intents: Server Members)

## Quick start

1) Copy the environment template and set at least your Discord token:

```bash
cp .env.example .env
$EDITOR .env
```

Required: `DISCORD_TOKEN`

Optional (recommended):

- `APPLICATION_ID` to easily generate an invite link
- `OWNER_ID` for owner-only commands
- `DATABASE_URL` (defaults already match the included Postgres service)

2) Start the stack:

```bash
docker compose up -d
```

3) Invite the bot to your server (replace with your Application ID):

```text
https://discord.com/oauth2/authorize?client_id=<YOUR_APP_ID>&scope=bot%20applications.commands&permissions=134217728
```

4) View logs (optional):

```bash
docker compose logs -f bot
```

## What’s included

- `docker-compose.yml`
  - bot: `nanashithenameless/namelessnamesanitizerbot:latest`
  - db: `postgres:18` with a healthcheck and a named volume (`db`)
  - default `DATABASE_URL=postgresql://bot:bot@db:5432/bot`
- `.env.example` with sensible defaults and documentation

Notes:

- Database storage persists in the named volume `db` by default. To bind-mount to the repo, uncomment the indicated line in `docker-compose.yml`.
- Host timezone is mounted read-only via `/etc/timezone` and `/etc/localtime`.

## Common operations

- Update to the latest image:

```bash
docker compose pull
docker compose up -d
```

- Stop the stack:

```bash
docker compose down
```

## Permissions and intents

- The bot needs “Manage Nicknames” to edit nicknames.
- For automatic sweeps and joins, enable “Server Members Intent”.

## Troubleshooting (quick)

- Commands not visible yet? Allow several minutes after first startup for global slash command sync and ensure application.commands scope.
- Not changing nicknames? Confirm the bot has “Manage Nicknames,” role order is correct, and the sanitizer is enabled per-guild.
- Database not ready? Wait for the Postgres healthcheck to pass; check logs and `DATABASE_URL`.

## Security & privacy

See `SECURITY.md` in this repo and the policies on the project site:

- Privacy Policy: https://namelessnamesanitizerbot.namelessnanashi.dev/PrivacyPolicy/
- Terms of Service: https://namelessnamesanitizerbot.namelessnanashi.dev/TermsOfService/

## License & Credits

See `license.md`.

- [All Major Contributors](./CONTRIBUTORS.md)
- [All Other Contributors](https://github.com/NanashiTheNameless/NamelessNameSanitizerBot/graphs/contributors)
