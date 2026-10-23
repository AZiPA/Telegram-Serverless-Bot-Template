# Contributing

This repo is a template people copy. A change here should teach the
platform, not grow the todo bot.

## Before you write code

Open an [issue](https://github.com/erfnzdeh/Telegram-Serverless-Bot-Template/issues)
if you are adding a handler type or changing the schema.

Things to keep:

- **Relative paths with explicit `.js` extensions.** The runtime requires
  relative imports with `.js` extensions. `import { addTodo } from '../lib/todos.js'`
  is correct; bare imports like `'lib/todos'` are no longer used.
- **Every query that can see another chat's rows must stay scoped by
  `chatId`.** See `tgcloud/lib/todos.js`. A toggle that keys only on `id` is a
  cross-tenant bug.
- **`push` never migrates.** Schema changes stay in `tgcloud/schema.js` until
  someone runs `npx tgcloud migrate`. Do not "help" by applying them in
  a deploy script.
- **Drops go through `.deprecated('reason')`.** Deleting a column from
  the file drops nothing.

There is no local runtime. `npx tgcloud run tgcloud/handlers/message '{...}'`
executes on the platform with a fake payload. CI only syntax-checks the
modules.

## Setup

```bash
npm install --no-package-lock
npx tgcloud login
npx tgcloud push
npx tgcloud migrate
```

`.tgcloud/` is CLI state and credentials. It is gitignored. Never commit
it, and never read it into a handler.

Read [AGENTS.md](AGENTS.md) before touching handlers or `tgcloud/schema.js`.
The SDK surface is [docs/tgcloud-sdk.md](docs/tgcloud-sdk.md).

## Pull requests

- One lesson per PR. A new handler type is a PR; rewriting the README
  around it is the same PR; a drive-by schema cleanup is not.
- Keep each folder's README true. The files exist to be read in order.
- Match the surrounding prose. No em dashes.
- Do not commit `.tgcloud/`, a bot token, `package-lock.json`, or `node_modules`.

## Surfaces

| Path | What it is |
|---|---|
| `tgcloud/schema.js` | Tables. Named exports. No foreign keys. |
| `tgcloud/handlers/` | One file per update type. Default export is the handler. |
| `tgcloud/lib/todos.js` | Queries plus the shared list renderer. |
| `tgcloud/lib/users.js` | Idempotent upsert. |
| `docs/tgcloud-sdk.md` | db, Bot API, fetch. |
| `AGENTS.md` | Short rules for humans and coding agents. |

Contributions are under the same MIT license as the rest of the repo.
