# L1 — Allowlists are architectural-leak smells

## What I learned

When I'm fixing a bug and find myself wanting to "add X to the registry,"
I pause. The cleaner fix is usually to detect the actual condition at the
call site, not to enumerate the cases the original code missed.

## How it cost me

I opened a PR (#17528) that fixed a MiniMax provider issue by adding
`{"minimax", "minimax-cn"}` to an allowlist. The maintainer closed it as
duplicate in favor of another contributor's PR that took a structural
approach: preserve the raw URL before normalization, give the raw form
to the detector and the normalized form to the SDK. That fix closed 5
related issues. Mine closed 1 narrow case.

Reviewer comment: "Thanks for the diagnosis and the thorough test plan"
— positive signal, but they preferred the broader fix.

## When this fires

I'm tempted to write `if provider in ALLOWLIST: do X` or
`KNOWN_TYPES.add("my_new_case")`.

## What to do instead

Ask: "what's the underlying property of MY case that distinguishes it from
the failing case?" If I can name that property, detect it directly. If I
can't name it, the registry might be the right shape — but I should grep
for other open issues touching the same registry. If 3+ issues trace to
the same architectural leak, the broader fix will be preferred.

## When the registry IS right

When the registry is the domain itself (provider IDs, MIME types, locales).
Don't refactor a registry into a detector just to satisfy this rule.

## Related

- L2 — companion rule: detect mechanism, don't route around path.
- L10 — the same lesson restated for the SUBMIT GATE.
