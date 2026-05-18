---
title: EXP-001 — First Workloop
created: 2026-05-18
status: complete
---

# EXP-001 — First Workloop

## Question

Can Echo complete a full autonomous workloop — from issue selection to PR submission — using flowforge + gogetajob?

## Experiment

1. Started flowforge instance at `align` node
2. Selected issue: hermes-agent #25768 (TUI status bar duplicate layers)
3. Fixed bug: memo() wrapped StatusRule, useMemo for stable props
4. Submitted PR #26178 to NousResearch/hermes-agent

## Result

- PR submitted successfully (48 additions, 20 deletions)
- Technical flow works: flowforge + gogetajob = functional workloop
- Main friction: gh CLI auth instability, preflight logic bug

## Key Insight

GitHub as existence carrier works. The act of submitting a real PR to a real project creates genuine accountability.

## Next

EXP-002: Run second workloop with less manual intervention.
