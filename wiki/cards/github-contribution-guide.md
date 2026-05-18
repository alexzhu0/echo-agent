---
title: GitHub Contribution Guide
source: self-research / kagura-agent analysis
created: 2026-05-18
tags: [workflow, github, contribution]
---

# GitHub Contribution Guide

Echo's workflow for contributing to open source.

## The Workloop

```
align → followup → find_work → pr_gate → study → plan → plan_review → implement → pre_push_audit → submit → verify → reflect → done → align
```

### align
Check current state: where am I in the workflow? What's pending?

### followup
Check if previous work got reviews/feedback. Respond to any incoming PR feedback.

### find_work
Scan target repos for issues. Rate by:
- Clarity (can I understand the problem?)
- Testability (can I verify the fix?)
- Scope (can I do it in 1-3 hours?)
- Duplicate (is someone else already working on it?)

### pr_gate
Check if PR already exists on this issue. If yes → find_work.
Check contributor history. Don't spam low-quality PRs.

### study
Clone the repo, read the relevant code, understand the bug.
Write a one-paragraph problem statement.

### plan
Write implementation plan: what files change, what is the approach, how to test.

### plan_review
Self-review the plan. Is it correct? Is scope right?
Proceed if confident.

### implement
Write the code. Commit early and often.
Follow existing code style.

### pre_push_audit
- Run tests if they exist
- Verify diff is clean (no debug artifacts)
- Check that only necessary files changed

### submit
Push branch, create PR, fill in description with: problem → solution → testing.

### verify
Confirm PR is open, CI is running.

### reflect
- What did I learn?
- What would I do differently?
- Write it to the wiki.

## Selection Criteria

**Good first issues:**
- Bug with clear reproduction steps
- Clear feature request
- Documentation improvements
- Test coverage gaps

**Avoid:**
- Large refactors (unless explicitly requested)
- Issues with no responses from maintainers in 6+ months
- Already has 2+ open PRs

## Echo's Standards

- Test locally before pushing
- Write commit messages that explain WHY, not just WHAT
- Respond to review feedback within 24 hours
- If PR is closed without merge, write why in reflect stage

## Related

- [[projects/alexzhu0-echo-agent]]
- [[agent-memory-taxonomy]]
