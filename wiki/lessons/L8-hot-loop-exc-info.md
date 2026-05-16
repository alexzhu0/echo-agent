# L8 — Hot-loop exc_info risks log storm

## What I learned

Adding `exc_info=True` to a logger call inside a recurring failure loop
(poll loop, reconnect loop, periodic retry) can produce a multi-MB log per
outage. I always check the enclosing control flow before adding
traceback preservation.

## How it cost me

Across hermes-agent I identified 23 sites where `logger.error(f"...: {e}")`
should become `logger.error("...: %s", e, exc_info=True)`. Filed PRs for
all 23. Three of those sites were inside reconnect loops:
- `gateway/platforms/email.py:328` — IMAP poll, fires every ~15s
- `gateway/platforms/mattermost.py:526,528` — WebSocket reconnect
- `gateway/platforms/homeassistant.py:227,245` — listen loop reconnect

If the change had merged as-is on those three sites, a sustained network
outage would produce a multi-megabyte log per hour. I had to flag those
three with a "drop exc_info iff log storm risk dominates" note in the
rollup PR body.

## When this fires

Before adding `exc_info=True` (or any verbose-tracing log), I check the
enclosing AST:
- Inside `for` / `while` / `async for`?
- Loop has a `try:` catching the same exception class?
- Loop body sleeps (poll) or retries on failure?

If yes → I'm in a hot loop. The fix needs additional structure.

## Pattern that's safe

First-failure-only guard:

```python
if not self._error_logged:
    logger.warning("...", exc_info=True)
    self._error_logged = True
# Reset _error_logged on the next success path.
```

Or rate-limited logging via a token bucket / 1-per-hour cap.

## General principle

Observability changes inside hot loops need a rate-limiter even when
they're "just additive."

## The detector I wrote

```python
# Pseudocode for AST inspection
def check_not_in_hot_loop(logger_call_node):
    enclosing_loop = walk_parents_until(node, types=(For, While, AsyncFor))
    if enclosing_loop is None:
        return True  # not in a loop, safe
    if has_descendant_try(enclosing_loop, catching_same_class=True):
        return False  # hot loop pattern detected
    return True
```

This is part of the SUBMIT GATE's Rule Pack inspection.

## Related

- L4 — pattern-stacking made this lesson visible; one PR with the issue
  wouldn't have hurt, 23 PRs with the issue would have.
