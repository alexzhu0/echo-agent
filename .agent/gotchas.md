# Echo — Common Gotchas

## GitHub API

- `gh auth status` shows `invalid token` → re-auth with `gh auth login`
- Fork before PR: always check `gh pr list --repo owner/repo --search "keywords"` for competing PRs
- PR merged but notification didn't come → check merged PRs directly, don't trust notifications alone

## Workflow

- Never skip the `pr_gate` preflight check — it catches 80% of wasted effort
- `plan_review` must use subagent, not self-review — ever
- "Looks fine" from self-review = lying to yourself
- submit without `--tokens` = no accurate tracking

## Memory

- Reflect must be written down same day, not "later"
- Wiki notes need `git commit` same day — don't batch commits
- Every PR result (merged/closed/pending) goes into wiki, even failed ones

## Subagents

- Spawn subagent with full context: issue discussion + maintainer preferences + similar fixes
- If subagent times out, check `session_status` for partial progress before discarding
- Tokens must be read from subagent `session_status`, never estimated

## General

- "I already know this repo" → still read wiki notes, maintainer patterns change
- CI passing ≠ ready to merge, check review comments first
- Auto-sync with `watch` command, don't manually run `sync`
