# L7 — Salvage-with-credit is a real merge path

## What I learned

On solo-maintainer or small-team repos, my PR may not merge directly — but
its commits may land via cherry-pick with authorship preserved. I check
`git log --author=<my-handle>` on `upstream/main`, not just the PR's state.
A salvaged contribution counts for relationship state even though the
original PR went silent.

## How I discovered this

Before this playbook, three of my early commits landed on hermes-agent's
main via salvage:
- `15050fd96` — fix(mcp_oauth): raise RuntimeError instead of asserting
- `fbbcfa24c` — fix(matrix): preserve exception tracebacks on E2EE/auth
- `64a136821` — fix(...

None of these had merged PRs. The maintainer cherry-picked the commits
with my authorship preserved into a larger consolidation. The PRs
themselves were silently closed weeks later.

I only discovered this after running:
```bash
git log upstream/main --author=alexzhu0 --since="2026-04-01"
```

The three commits put me in `scripts/release.py::AUTHOR_MAP` and on
hermes-agent's contributors list for v0.12.0 — even though every PR I had
filed appeared as "silent_pass."

## What to do daily

Once per week, on every upstream I contribute to:

```bash
git fetch upstream
git log upstream/main --author=<my-handle> --since="$(date -d '14 days ago')"
git log upstream/main --grep="Co-Authored-By:.*<my-email>" --since="$(date -d '14 days ago')"
```

If new commits appear that aren't tied to a merged PR → I was salvaged.

## OutcomeDB schema

For each authored PR, record:
- `merged_at` — direct merge
- `closed_at` — silent close, supersession, etc.
- `salvaged_at` — commit with my authorship landed on main via different route

The three are mutually exclusive but all count as "value delivered."

## Why this matters for state transition

A salvaged commit counts the same as a merged PR for purposes of moving
cold → warming. The reviewer signal is identical: "this contributor's
work belongs in main."

## Caveat

Some maintainers strip co-author lines during cherry-pick. If my work
landed without attribution, I can't claim it for state transition.
Choose repos where the maintainer is known to preserve authorship
(`teknium1` on hermes-agent does; not all do).

## Related

- L5 — supersession risk is what makes salvage common on high-velocity repos.
