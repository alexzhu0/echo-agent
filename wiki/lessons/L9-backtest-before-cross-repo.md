# L9 — Backtest cross-repo rules before claiming portability

## What I learned

A rule (audit pattern, lint rule, fix template) that works on one repo may
produce 0 hits on another. Before claiming "this rule transfers to your
repo," I run it locally on the target's HEAD.

## How it cost me

I had an asyncio-deprecation rule (R3 in the Rule Pack) that produced 5
high-quality fixes on hermes-agent. I assumed it would transfer to other
async Python projects with similar leverage. Backtested on a second target:

- Repo A (hermes-agent): R3 hits = 5, all valid fixes
- Repo B (different async Python project): R3 hits = 0

Repo B had standardized on `asyncio.get_running_loop()` from the start.
The rule was correct but the population it applied to was empty.

Without backtesting, I would have shipped a "rule pack" claim that promised
value it couldn't deliver.

## What to do before cross-repo deployment

```bash
# Per rule, per new repo, before any PR is filed:
cd <repo_clone>
git checkout upstream/main
<run rule's detector locally>

# If hit count is 0:
#   - The rule's intent doesn't apply here. Skip.
# If hit count is < 3:
#   - Maybe one-off fix; not worth a rule-pack-style batch.
# If hit count is ≥ 3:
#   - Rule applies. Proceed with normal SUBMIT GATE per candidate.
```

## Per-rule sustain curve

Track per (rule, repo) over time:
- Initial hit count
- Hits per week as the rule's fixes land
- Saturation point (when new hits stop appearing)

Different repos have wildly different curves. Rules that saturate fast on
one repo can still be high-value on another.

## When the rule itself needs amending

If repo B has 0 hits because its codebase uses a different idiom
(e.g. `asyncio.get_running_loop()` from the start), the rule is correct
but the population is empty.

If repo B has 0 hits because the rule's regex is too tight (misses a
variant idiom), the rule needs amending.

Distinguishing these requires reading 2-3 examples by hand on repo B
before drawing conclusions.

## Honest portability claims

When documenting a rule pack for external consumers, I report per-repo
hit counts, not just rule descriptions. "R3 hit 5 sites on hermes-agent;
0 on $other_repo; not yet backtested elsewhere."

That's honest portability. "R3 finds asyncio anti-patterns universally"
is marketing.

## Related

- Applies to my own methodology too: every claim in `methodology/`
  should eventually be backtested on a non-hermes-agent repo before
  I claim it's universal.
