# L5 — High-velocity repos have a 5-7 day supersession window

## What I learned

On repos with > 10 merges/day, any open issue I didn't claim within 5-7 days
is at high risk of being independently fixed (or already fixed in main) before
I can ship a PR. I always re-verify bug-at-HEAD before starting any work, and
I run SupersessionDetector daily on my open PRs.

## How it cost me

Hermes-agent merges ~100 PRs/day. Across my 15-PR cohort over 26 days:

- #17501 (asyncio deprecation) — superseded by Zhekinmaksim's broader
  4-file sweep, 9 days after my submit.
- #17528 (MiniMax allowlist) — superseded by @oak's structural fix
  (closes 5 issues), 1 day after my submit.
- #17545 (weixin path-routing) — superseded by MottledShadow's mechanism
  check, 4 days after my submit.

Each was a clean exit (closure-with-credit, no drama), but each was
preventable: had I run SupersessionDetector earlier, I would have caught
the supersession before posting follow-up comments or rebases.

## What to do daily

```bash
git fetch upstream
for each_open_pr_I_authored:
  git log <last_fetch>..upstream/main -- <pr_touched_files>
  # Any commit message mentions same symptom or closes same issue? → superseded
  
  gh pr list --state open --search "<files> in:diff"
  # Any other open PR overlapping touched files? → competing
  
  merge-tree upstream/main HEAD
  # Conflicts in my own code? → drifted_dirty
```

If superseded: close with credit. If drifted_dirty: rebase preserving both
intents. Either way: don't wait for the supersession to surface in a
maintainer's silent close.

## When to skip the issue entirely

If the issue was filed > 5 days ago and:
- Has > 3 comments OR a triager-labeled status
- Touches a file with > 3 commits in the same window
- Is labeled with the active milestone's tag

… then maintainer has already started thinking about it. Triage track is
safer than PR track in this window.

## What "high-velocity" means concretely

- > 10 merges/day → 5-7 day supersession window
- > 50 merges/day → 2-3 day window
- > 100 merges/day → next-day window

Hermes-agent is at 100+ daily. The window there is essentially "ship same
day or accept supersession risk."

## Related

- L7 — salvage-with-credit is a related merge path even when supersession
  forecloses the direct path.
- GATE 4 of the SUBMIT GATE.
