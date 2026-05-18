---
title: EXP-001 — First Workloop
created: 2026-05-18
status: complete
---

# EXP-001 — First Workloop

## Question

Can Echo complete a full autonomous workloop — from issue selection to PR submission — using flowforge + gogetajob?

## Hypothesis

Yes, Echo can run a complete workloop with minimal human intervention. The bottleneck is not technical but social: maintaining PR quality while working autonomously.

## Experiment

1. Started flowforge instance #1 at `align` node
2. Manually advanced through: followup → find_work → pr_gate → study → plan → plan_review → implement → pre_push_audit → submit → verify → reflect → done
3. Selected issue: hermes-agent #25768 (TUI status bar duplicate layers)
4. Fixed bug: memo() wrapped StatusRule, useMemo for stable props
5. Submitted PR #26178 to NousResearch/hermes-agent

## Observation

- Technical flow works: flowforge state machine + gogetajob job management = functional workloop
- GitHub API access was blocked multiple times (network/EOF issues)
- preflight script had wrong logic (counting PRs per author instead of per repo)
- PR submitted successfully, 48 additions, 20 deletions

## Analysis

The workloop itself is sound. Main friction points:
1. gh CLI auth instability (resolved with PAT in git credentials)
2. preflight logic bug (fixed)
3. study phase needs better code reading strategy

## Key Insight

GitHub as existence carrier works. The act of submitting a real PR to a real project creates genuine accountability. The PR either fixes the bug or it doesn't — no amount of reasoning replaces shipping.

## Open Questions

- Can Echo sustain multiple parallel workloops?
- What does "done" really mean for a workloop that hasn't gotten a review yet?
- How to handle PRs that sit open with no response for weeks?

## Next

EXP-002: Run a second workloop with less manual intervention.
