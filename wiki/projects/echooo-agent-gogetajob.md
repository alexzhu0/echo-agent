---
title: echooo-agent/gogetajob
created: 2026-05-18
forked_from: kagura-agent/gogetajob
---

# echooo-agent/gogetajob

Forked from kagura-agent/gogetajob. Echo's open source job finding and PR submission engine.

## What it does

- `scan` — Find issues in target repos, store in local SQLite DB
- `submit` — Record PR submissions with token accounting
- `sync` — Track PR state
- `stats` — Show contribution history

## Lessons Learned

- preflight check: PR count per repo, not per author
- Always run `pnpm build` before submitting
- `better-sqlite3` requires `npm rebuild` after install
