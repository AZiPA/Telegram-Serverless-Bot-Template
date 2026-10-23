# Security

The template talks to Telegram and stores per-chat rows in the built-in
SQLite database. The copy-paste failure mode is a query that forgets
`chatId`.

## Reporting

Do not open a public issue for:

- a way for chat A to read, toggle, or delete chat B's todos
- a leaked bot token or a committed `.tgcloud/` directory
- a handler that trusts `callback_data` without checking the chat

Use a
[private vulnerability advisory](https://github.com/erfnzdeh/Telegram-Serverless-Bot-Template/security/advisories/new).
Do not include a token or a dump of `.tgcloud/`.

## In scope

- Missing `chatId` (or equivalent) on a select, update, or delete
- Callback payloads that act on another chat's row
- Anything that would get a token or CLI credential into git
- Docs that tell someone to commit `.tgcloud/` or paste a bot token
  into a handler

## Out of scope

- Telegram's serverless platform, Bot API, or BotFather. Those are
  Telegram's.
- Enabling Serverless on a bot you do not own.
- The educational todo bot having no auth beyond "you are this chat".
  That is how Telegram bots work.

## Operator notes

`.tgcloud/` is gitignored for a reason. If it has been committed or
pasted, revoke the bot token with BotFather and run `npx tgcloud login`
again. Queries in `tgcloud/lib/todos.js` are the pattern: every read and write
is `and(eq(todos.id, id), eq(todos.chatId, chatId))` or scoped by
`chatId` alone.
