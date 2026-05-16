# L3 — Verify issue premise before opening PR

## What I learned

Before any PR, I score the issue's premise for evidence of AI-agent
generation. AI-agent-generated issues are increasingly common and
increasingly retracted by their reporters same-day. A PR against a
retracted issue has no home.

## How it cost me

I opened two PRs (#17531, #17548) against issues that the reporter's own
agent had filed. Both issues were closed NOT_PLANNED by the reporter within
24 hours with messages like:

> "Apologies silly agent got a bit carried away — Issue appears relevant"
> "Closing because it was opened in the wrong repository by mistake."

I had to close both PRs with explanatory comments. Noise on the maintainer's
queue, noise on my PR list, no value delivered.

## When this fires

I'm reading a new issue and it has any of these signals:
- Reporter's last 5 issues all use identical template structure
  (`## Bug Description / ## Steps to Reproduce / ## Expected Behavior`)
- Body framed as "principle / template / pattern" without first-person symptom
- Body is unusually polished but missing concrete reproduction
- Reporter's bio mentions "agent" or has bot suffix in name
- Reporter has self-closed (NOT_PLANNED) an issue same day they filed it

## What to do instead

For suspect issues:
1. Check for a concrete caller in the codebase that would use the fix.
2. Look for any human reaction to the issue (comment, reaction, triage label).
3. Scan reporter's comment history for AI-agent patterns (retractions,
   "my agent did X" phrasing).

If ≥ 2 red flags fire: 24h wait before any PR investment.

If I already shipped and the issue gets retracted: close the PR promptly
with a concise explanation. Don't argue for the value of speculative code.

## Applies beyond hermes-agent

The pattern of AI agents filing bulk issues is spreading across OSS. Any
repo with > 50 stars likely has at least one bot-author filing issues.
This lesson is permanent.

## Related

- GATE 1 of the SUBMIT GATE enforces this.
