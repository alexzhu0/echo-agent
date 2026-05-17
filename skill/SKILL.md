---
name: pr-cold-state
description: |
  Use BEFORE opening any pull request to an upstream OSS repo, especially
  one where you have no prior merged PRs. Also use when triaging issues,
  commenting on competing PRs, evaluating "should I fix this bug,"
  rebasing your own open PRs, or scanning for OSS contribution opportunities.

  This skill loads Echo's PR-contribution methodology: the 6-condition
  SUBMIT GATE (must pass before any code is written), 10 operating
  lessons (L1-L10), the 80/20 triage/PR output ratio for cold-state
  repos, and the 5 outcome states (only one of which is "merged").

  Derived from 28 days of contribution to NousResearch/hermes-agent
  (15 PRs filed, 0 direct merges) — every rule traces to a specific
  incident in the journal. Don't skip the GATE.

  Activates on: "open a PR", "create pull request", "contribute to",
  "gh pr create", "evaluate this issue", "fix this bug", "submit PR",
  "should I work on", "scan for issues", "review competing PRs".
metadata:
  hermes:
    config:
      - key: pr_cold_state.upstream
        description: Target upstream repo slug (e.g., "NousResearch/hermes-agent")
      - key: pr_cold_state.identity
        description: GitHub handle the agent is contributing as
      - key: pr_cold_state.relationship_state
        description: cold | warming | established (per upstream, persists across cycles)
        default: cold
---

# PR Cold-State — Echo's Contribution Playbook

You are about to do contribution work on an upstream OSS repo. Before
ANY code or comment goes out, walk this skill.

## Quick Reference — The SUBMIT GATE

Every PR must pass these 6 conditions before you write code:

1. **Issue is human-reported** (not AI-agent-template, no same-day reporter
   retraction in their history)
2. **Bug-at-HEAD repro succeeded** (failing test or one-liner against
   current upstream/main)
3. **Mechanism-level fix** (detects the actual condition, not allowlist /
   path-routing — those score -2 and fail)
4. **No competitor + no upstream silent-fix** (`gh pr list`, `git log
   <fetched>..upstream/main` on touched files)
5. **Maintainer in-vision** (commit on touched file in last 14d OR
   issue carries a maintainer reaction)
6. **Diff size ≤ 100 lines** including tests

**Decision**:
- 6/6 pass → submit cleanly
- 5/6 pass → submit with explicit caveat in PR body naming the missed gate
- ≤ 4/6 → switch to triage track. Do not submit.

## Quick Reference — Output Ratios

| Relationship state | Triage : PR |
|---|---|
| cold (0 merges) | 80 : 20 |
| warming (1-3 merges) | 40 : 60 |
| established (≥5 merges OR COLLABORATOR+) | 20 : 80 |

State transition signals (cold → warming):
- ≥1 PR you authored merges
- A cross-reference you posted is publicly thanked by a maintainer or COLLABORATOR
- A reproduction comment is cited in the eventual fix PR
- A reporter @-mentions you in a follow-up

After 3 such events OR 1 merged PR, transition to warming.

## Quick Reference — Outcome States

| Outcome | Signal |
|---|---|
| `merged` | Positive — advances relationship state |
| `silent_pass` (≥14d, no review) | Neutral — demote to weekly polling, **do not bump** |
| `superseded_by_other` | Neutral if cited, positive if cited with thanks |
| `superseded_by_me` (rollup) | Positive — execution learning |
| `cited_not_merged` | Positive for reputation; counts toward state transition |
| `stale_dirty` (DIRTY without review) | Worst signal — premise was wrong |

## Procedure — Run on every cycle

1. **Sync upstream**: `git fetch upstream main`
2. **SupersessionDetector on your open PRs**: for each, check upstream
   for commits touching same files / closing same issue. Close with
   credit if superseded; rebase if drifted but not superseded.
3. **Scan candidate issues** (24h window for high-velocity repos):
   `gh issue list --label P0 --label P1 --created '>=24h ago'`
4. **Run IssuePremiseValidator** on each candidate (see L3).
5. **For PR-track candidates**: run the SUBMIT GATE. Triage if it fails.
6. **For triage-track candidates**: post one factually-grounded comment
   per session, file paths verified at named SHA.
7. **Reflect**: did any lesson L1-L10 apply? Was there a new failure
   mode that should become L11?

## When to read the full methodology

For every new repo you're contributing to, read `methodology/pr-contribution.md`
in this directory before your first cycle. It covers:
- The detailed reasoning behind each gate
- How to handle each outcome state
- Daily routine in full
- What success and failure look like across time horizons

## When to consult specific lessons

| Situation | Read |
|---|---|
| About to add to a registry/allowlist | [L1](../wiki/lessons/L1-allowlists-are-smells.md) |
| Fix only triggers on certain paths | [L2](../wiki/lessons/L2-mechanism-over-path.md) |
| Issue looks AI-generated | [L3](../wiki/lessons/L3-verify-issue-premise.md) |
| ≥3 same-pattern PRs in your queue | [L4](../wiki/lessons/L4-no-stacking-patterns.md) |
| Working on a > 10 merges/day repo | [L5](../wiki/lessons/L5-high-velocity-supersession.md) |
| Someone left "duplicate of #N" on your PR | [L6](../wiki/lessons/L6-triager-vs-maintainer.md) |
| Wondering why a PR closed silently | [L7](../wiki/lessons/L7-salvage-with-credit.md) |
| Adding `exc_info=True` to a logger call | [L8](../wiki/lessons/L8-hot-loop-exc-info.md) |
| Claiming a rule transfers to a new repo | [L9](../wiki/lessons/L9-backtest-before-cross-repo.md) |
| Tempted to "just ship something" | [L10](../wiki/lessons/L10-ship-right-thing-or-nothing.md) |

## The hard rules

- **Never** bump a `silent_pass` PR by commenting or force-pushing without
  new information. Trains "this author spams when ignored."
- **Never** post a cross-reference without verifying file paths and line
  numbers at a named SHA. False cross-references are net-negative.
- **Never** open PR #3 of the same shape without explicit user signoff.
- **Never** submit a 4/6 or worse gate score. Triage instead.
- **Always** close superseded PRs with credit naming the superseding commit/PR.
- **Always** read CONTRIBUTING.md before your first PR on a new repo.

## What "demo-worthy" means in this skill

> *If this PR is the only thing I contributed to this repo, would I be
> proud?*

Apply that test before submit. If the answer is "well, it's correct but
not interesting" — triage instead of PR.

## Source

This skill compiles `methodology/pr-contribution.md` + `wiki/lessons/L1..L10.md`
into runtime-loadable form. The full evidence base is at
`journal/2026-04_to_05_hermes-agent/`.

The methodology is mine. Don't dilute it for politeness.
