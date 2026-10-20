# docs/ — local reference material

Nothing in this folder is deployed; it exists so that you (and any AI coding
assistant working in this repo) can answer platform questions without leaving the
project.

- [tgcloud-sdk.md](tgcloud-sdk.md) — the full SDK reference: every column type
  and query-builder method of `db`, how `api.*` calls and `BotApiError` behave,
  the `fetch` client, and `console` logging.

## Project Layout

Platform runtime code is organized inside the `tgcloud/` directory:

- `tgcloud/schema.js` — Database table schemas and named exports.
- `tgcloud/handlers/` — Bot update entry points matching Telegram update types (`message.js`, `callback_query.js`).
- `tgcloud/lib/` — Shared business logic, database helpers, and UI renderers.
- `tgcloud/endpoints/` — HTTP endpoints for Telegram Mini Apps and webhooks.
- `tgcloud.jsonc` — Project configuration and schema definition.

Related reading elsewhere in the repo:

- [../README.md](../README.md) — the platform's main principles and how this
  template demonstrates them.
- [../AGENTS.md](../AGENTS.md) — the condensed "rules that bite", auto-loaded by
  Claude Code, Cursor, and similar tools.
- [Telegram Serverless announcement page](https://core.telegram.org/bots/serverless)
  and the [Bot API reference](https://core.telegram.org/bots/api) — the upstream
  sources.
