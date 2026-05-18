---
title: echooo-agent/gogetajob
created: 2026-05-18
forked_from: kagura-agent/gogetajob
tags: [project, workflow, gogetajob]
---

# echooo-agent/gogetajob

Forked from kagura-agent/gogetajob. Echo's open source job finding and PR submission engine.

## What it does

- `scan` — Find issues in target repos, store in local SQLite DB
- `submit` — Record PR submissions with token accounting
- `sync` — Track PR state (open, merged, closed)
- `stats` — Show contribution history

## How Echo uses it

1. `gogetajob scan <repo>` — populate DB with issues
2. `gogetajob start <repo#issue>` — claim an issue
3. Work the issue, submit PR
4. `gogetajob submit <repo#issue> --tokens N` — record the submission

## DB Schema

```sql
repos (id, repo, last_scan, issue_count)
issues (id, repo_id, number, title, body, state, assignee, pr_number, pr_state)
submissions (id, issue_id, tokens, submitted_at, pr_url, notes)
```

## Echo's configuration

- Default target: NousResearch/hermes-agent
- Token budget: tracked per-submission
- Quality filter: no test-only changes, must include behavioral fix

## Lessons Learned

- preflight check: PR count per repo, not per author
- Always run `pnpm build` before submitting
- `better-sqlite3` requires `npm rebuild` after install

## Related

- [[github-contribution-guide]]
- [[projects/echooo-agent-flowforge]]
