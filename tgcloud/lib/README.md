# tgcloud/lib/ — shared modules

Everything that isn't an entry point lives here: database access, rendering,
validation, API wrappers. Unlike `handlers/`, this folder allows subdirectories
(`tgcloud/lib/internal/util.js`), and file names carry no special meaning — a lib module
runs only when another module imports it.

## Importing — the rule that bites

Modules are addressed via **relative paths with explicit `.js` extensions**:

```js
import { addTodo } from './todos.js';      // ✅ relative path with .js
import { users } from '../schema.js';      // ✅ schema imported from lib/
import { db, api } from 'sdk';             // ✅ the platform SDK
import { eq, sql } from 'sdk/db';          // ✅ platform SDK submodules

import { addTodo } from 'lib/todos';       // ❌ bare module names are no longer used
import { addTodo } from './todos';         // ❌ missing .js extension
import _ from 'lodash';                    // ❌ no npm packages at runtime
```

The runtime resolves local modules by their relative path with `.js`, and SDK modules via `'sdk'` / `'sdk/db'`.

## How the demo splits responsibilities

- [todos.js](todos.js) — **all** todo database access, plus `renderList()`, the
  one function that turns rows into `{ text, reply_markup }`. Both handlers call
  it, which is exactly why it lives here: the message handler renders the list
  when you add a todo, and the callback handler re-renders it after a button tap.
  If each handler built its own keyboard, the two would drift apart — and the
  `callback_data` strings the keyboard embeds (`toggle:<id>`, `delete:<id>`) are
  a contract with `tgcloud/handlers/callback_query.js`, so they should be written in
  exactly one place.
- [users.js](users.js) — the smallest useful pattern: an idempotent upsert
  (`onConflictDoUpdate`) that keeps one row per Telegram user fresh on every
  message.

The guideline the demo follows: **handlers decide, lib does.** A handler parses
the update and picks a branch; the queries, the keyboard layout, and any
cross-handler contract live here. That keeps handlers readable top-to-bottom and
makes logic testable via `npx tgcloud run` with different payloads.

Note that `lib/todos.js` scopes every query by `chatId`, including toggle and
delete — the ids arriving in `callback_data` are user-supplied input, and the
platform has no foreign keys or row-level security to catch a forged id. Guarding
ownership in the query itself is the pattern to copy.

> Only `.js` files deploy — this README stays local.
