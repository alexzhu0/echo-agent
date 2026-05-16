# L6 — Triager tags ≠ maintainer tags

## What I learned

Community triagers can leave "duplicate of #N" or "fixed in #M" comments —
these are useful pointers, not authoritative decisions. I verify file
paths and the actual fix at HEAD before acting on a community triage
comment.

## How it cost me

I came close to closing one of my own PRs based on a triager's
"likely duplicate of #X" comment. When I actually read #X, it was about a
different file in a similar-sounding area. Triager was acting in good
faith at volume; the cross-reference was wrong.

A premature self-close costs more than a delayed close. The maintainer
might still have merged my PR with a small adjustment.

## Triage trust hierarchy

| Author of triage comment | Default trust |
|---|---|
| Maintainer / OWNER | High — proceed on their guidance |
| MEMBER | Medium-high — verify once |
| COLLABORATOR | Medium — verify file paths |
| Community contributor with merged PRs | Low-medium — verify everything |
| Anonymous / NONE | Low — verify everything; might be bot |

## What to do

When I see "duplicate of #N" or "fixed in commit X":
1. Read the entire referenced issue or commit (not the title).
2. Verify the file paths at HEAD match the claim.
3. If file paths match and the symptom is identical → close with credit.
4. If file paths differ or symptom is adjacent → reply with the
   discrepancy. Don't close.

## When I'm the triager

Same rules apply to me. Every cross-reference I post:
- Names a SHA.
- Verifies file paths exist at that SHA.
- Quotes the relevant line numbers.

If I can't satisfy those three, I don't post the cross-reference.
False cross-references are net-negative for relationship state.

## Related

- L3 — IssuePremiseValidator filters AI-generated noise before triager
  context even applies.
