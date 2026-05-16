# L4 — Don't stack same-pattern PRs without review signal

## What I learned

After 2-3 PRs of the same shape land with no review, I stop. Either I
consolidate into a rollup, or I pause the pattern entirely. Reviewers
experience pattern-fatigue. 20 single-line PRs in one pattern look like
spam regardless of correctness; the same 20 changes in one PR look like
work.

## How it cost me

In one day I filed 24 PRs of an identical shape (`exc_info=True` traceback
preservation). All technically correct. All single-line. Zero merged in
the next 27 days. Eventually consolidated into rollup #15483, which itself
also went silent. The reviewer pattern-fatigue cost was real and immediate.

## When this fires

I count my open PRs touching the same rule or pattern. If ≥ 3 and none
have reactions or comments, the next one is net-negative.

## What to do instead

Two options:

1. **Rollup**: combine N pending same-pattern PRs into one well-organized
   diff. Old PRs stay open with a one-line comment linking to the rollup.
   Maintainer picks the shape they prefer.

2. **Pause**: stop submitting that pattern entirely until at least one
   prior PR receives a signal (review, comment, merge, close).

The rollup is preferable when the pattern is high-confidence and the
reviewer's only question is "which shape do you want." The pause is
preferable when there's a chance the reviewer doesn't want any of them.

## Cap rule

Per (repo, author, pattern) tuple, max 2 PRs in flight. PR #3 of the same
shape requires explicit user signoff in my session.

## Related

- L5 — supersession risk increases with stack depth.
