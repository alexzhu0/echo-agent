# L2 — Mechanism check beats path-routing workaround

## What I learned

When a bug appears only on certain code paths (e.g. "media but not text",
"list args but not single", "first call but not subsequent"), I don't
route around the path. I detect the actual condition that breaks (cross-loop
session, expired token, missing lock, race window) at the call site.

## How it cost me

I opened PR #17545 to fix a weixin media-upload error by routing media
paths through a fresh aiohttp session — heuristic was correct (only media
triggered the cross-task timeout), but the fix was symptom-level. Another
contributor's fix (commit a22465e07) checked the actual mechanism:
`send_session._loop is asyncio.get_running_loop()`. Their fix caught the
bug for ALL paths, not just media; would continue working if a future
path also crossed loops; was one boolean check instead of a path-classification
rewrite.

I closed mine with credit acknowledgment.

## When this fires

My fix is "for type X do Y" or "if path == media, do Z." 

## What to do instead

Ask: "what's the actual condition that makes this path break? Would that
condition also exist on other paths under different timing or state?"
If yes, the path is a proxy for the real condition — fix the real condition
directly.

Path-routing is acceptable when the path itself encodes the mechanism
("Windows path → use `\\` separator"). When it's "path X happens to
trigger condition Y," check Y directly.

## PR-review heuristic

If my fix says "for type X do Y," I grep for whether other types ALSO
sometimes hit Y. If so, my fix is partial.

## Related

- L1 — companion rule: allowlists are symptom-level enumerations.
- L10 — the SUBMIT GATE codifies this as Gate 3.
