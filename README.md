# NamelessNameSanitizerBot - Minimal Docker Runner [![Ask DeepWiki](<https://deepwiki.com/badge.svg>)](<https://deepwiki.com/NanashiTheNameless/NamelessNameSanitizerBot-Docker>)

This repository is a minimal, fast-to-run Docker setup for NamelessNameSanitizerBot. It contains only what you need to run the published Docker image with PostgreSQL via Docker Compose - no application source code.

If you're looking for the bot's source code, feature list, and full documentation, see:

- Full repo and docs: <https://github.com/NanashiTheNameless/NamelessNameSanitizerBot>
- Install to a server: <https://nncl.namelessnanashi.dev/install/>

## Why this repo exists

- Minimal footprint: just `docker-compose.yml`, `.env.example`, and a couple of docs
- Zero local build: uses the published image `ghcr.io/nanashithenameless/namelessnamesanitizerbot:latest`
- One command to start: bring up the bot and a PostgreSQL instance quickly

## Requirements

- Docker Engine and Docker Compose plugin
- Discord Bot token (with Bot scope; recommended intents: Server Members)

# Quick start

### 1. Clone the repository

```bash
git clone --filter=blob:none https://github.com/NanashiTheNameless/NamelessNameSanitizerBot-Docker.git \
&& cd NamelessNameSanitizerBot-Docker \
&& git sparse-checkout init --no-cone \
&& git sparse-checkout set '/TermsOfService.md' '/SECURITY.md' '/PrivacyPolicy.md' '/README.md' '/LICENSE.md' '/docker-compose.yml' '/.env.example' '/autoConfig.sh'
```

### 2. Create `.env`

Option A - automated (recommended)

```bash
./autoConfig.sh
```

The wizard prompts for your `DISCORD_TOKEN`, validates it, and generates secure Postgres credentials. It writes `.env` for you. Re-run to overwrite if needed.

Option B - manual

```bash
cp .env.example .env
# Open .env and set at minimum:
#   DISCORD_TOKEN=your-discord-bot-token
# Optional but useful:
#   OWNER_ID=your-discord-user-id
#   APPLICATION_ID=your-application-client-id (prints invite URL on startup)
```

Notes:

- The default `DATABASE_URL` already matches the provided Postgres service in this Compose stack.
- Keep secrets in local `.env`; do not commit it.

### 3. Start

As a daemon

```bash
docker compose up -d
```

In the terminal

```bash
docker compose up
```

### 4. Invite the bot to your server (replace with your Application ID)

```text
https://discord.com/oauth2/authorize?client_id=<YOUR_APP_ID>&scope=bot%20applications.commands&permissions=134217728&integration_type=0
```

## What's included

- `docker-compose.yml`
  - bot: `ghcr.io/nanashithenameless/namelessnamesanitizerbot:latest`/`nanashithenameless/namelessnamesanitizerbot:latest`
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

- The bot needs "Manage Nicknames" to edit nicknames.
- For automatic sweeps and joins, enable "Server Members Intent".

## Troubleshooting (quick)

- Commands not visible yet? Allow several minutes after first startup for global slash command sync, and ensure the application.commands scope.
- Not changing nicknames? Confirm the bot has "Manage Nicknames," the role order is correct, and the sanitizer is enabled per guild.
- Database not ready? Wait for the Postgres health check to pass; check logs and `DATABASE_URL`.

## Security & privacy

See [SECURITY.md](<./SECURITY.md>) in this repo and the policies on the project site:

- The bot does not log message content and doesn't require the Message Content intent.
- Logging channel (if set) only receives a short notice when a nickname is changed.
- Minimal data storage: per-guild config and per-user cooldown timestamps. Cooldowns are purged automatically after COOLDOWN_TTL_SEC.
- Users can request deletion via /delete-my-data; bot owners can execute /delete-user-data or /global-delete-user-data when legally required.

Public telemetry/census:

- Aggregate, non-identifying census metrics are published at <https://telemetry.namelessnanashi.dev/> for transparency.

Related policies:

- [Privacy Policy](<https://nncl.namelessnanashi.dev/PrivacyPolicy/>)
- [Terms of Service](<https://nncl.namelessnanashi.dev/TermsOfService/>)

## License & Credits

See [LICENSE.md](<./LICENSE.md>).

- [All Major Contributors](<./CONTRIBUTORS.md>)
- [All Other Contributors](<https://github.com/NanashiTheNameless/NamelessNameSanitizerBot-Docker/graphs/contributors>)

## AI agents

If you're using an AI assistant to help with this repo, see:

- [AGENTS.md](<./AGENTS.md>) - general guidance for LLM agents working in this repo
- [CLAUDE.md](<./CLAUDE.md>) - provider-specific notes for Anthropic Claude
- [COPILOT.md](<./COPILOT.md>) - provider-specific notes for GitHub Copilot in VS Code
