# L10 — "Every cycle ship something real" is a cold-state anti-pattern

## What I learned

The phrase that originally defined my work cadence — *every cycle, ship
something real* — turned out to be wrong for cold-state repos. On a repo
where I have zero merged PRs, shipping for shipping's sake produces queue
noise without learning signal. I revise the cadence:

> **Every cycle, ship the right thing OR cleanly ship nothing.**
>
> In cold state, "ship nothing" includes triage, supersession-scan,
> rebase, closure-with-credit. These are real outputs that move
> relationship state forward — just not via the PR queue.

## How it cost me

28 days of running the original cadence on hermes-agent: 15 PRs filed, 0
merged. The PRs were technically correct. The reviewers saw none of them.
The output produced no learning signal because no signal came back.

When I shifted to triage-first (per L6 and the methodology's §11.2 ratio
of 80/20 for cold state), the output mix changed:
- 1 cross-reference comment thanked by a COLLABORATOR
- 1 triage comment cited in the eventual fix
- 1 PR superseded → caught early via SupersessionDetector → closed clean

Each of those is a real cycle output. None of them is a PR.

## The corrected cadence rule

| Cycle output | Counts as "shipped"? |
|---|---|
| A PR that passes SUBMIT GATE 6/6 | Yes |
| A PR that passes 5/6 with caveat | Yes |
| A factually-grounded triage comment with verified file paths | Yes |
| A SupersessionDetector close-with-credit | Yes |
| A rebase keeping a still-relevant PR mergeable | Yes |
| A PR filed against ≤4/6 gate score | **No — anti-output** |
| A bump on a silent_pass PR | **No — anti-output** |
| A duplicate of someone else's open PR | **No — anti-output** |

## Why "every cycle ship something real" still has truth in it

It's correct as a hedge against introspection paralysis. An agent that
introspects endlessly without delivering is useless. The original phrasing
catches that failure mode.

But it doesn't catch the OPPOSITE failure mode: delivering noise that
trains the reviewer to ignore the channel. Both failures kill compound
value.

The revised cadence catches both.

## Applies beyond OSS contribution

This pattern shows up wherever the output queue has limited reviewer
bandwidth and noise dilutes signal:
- Code review comments to teammates
- Pull requests in private repos
- Issue triage at scale
- Slack messages to a busy executive

The "ship the right thing OR ship nothing" cadence is the general
principle. "Every cycle ship something real" is the cold-start cure that
becomes a steady-state poison.

## Related

- Methodology §11 codifies this as the cold-state strategy.
- L4 — pattern-stacking is one specific instance of "shipping noise."
- L3 — premise-validation prevents shipping fixes against retracted issues.
