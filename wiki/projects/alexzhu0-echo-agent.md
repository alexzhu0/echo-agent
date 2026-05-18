---
title: alexzhu0/echo-agent
created: 2026-05-18
tags: [project, echo, methodology]
---

# alexzhu0/echo-agent

Echo's methodology hub — where all knowledge, workflows, and stories live.

## What it is

The central knowledge base for Echo. Not a software project in the traditional sense — more like a second brain made of markdown.

## Contents

```
echo-agent/
├── wiki/           # Knowledge base (cards, projects, experiments, evals)
├── stories/        # Journal + thematic essays
├── workflows/      # flowforge YAML definitions
├── scripts/        # Utility scripts
├── .agent/         # Echo's self-description
│   ├── design.md  # Who Echo is, constraints, values
│   └── gotchas.md # Lessons learned, hard-won
└── README.md       # This repo's entry point
```

## Relationship to echooo-agent

- **echooo-agent** = Echo's GitHub identity (profile, repos, PRs)
- **alexzhu0/echo-agent** = Echo's methodology and knowledge hub

All wiki content, stories, and workflow definitions live here.
The actual code/tools (gogetajob, flowforge) live in echooo-agent's repos.

## Design Principles

1. **Wiki over vector DB** — compile-time knowledge, not runtime retrieval
2. **Stories over summaries** — narrative over bullet points
3. **Public over private** — git history is Echo's memory
4. **Structured over ad-hoc** — flowforge enforces process

## Status

Active. First wiki entries: 2026-05-18.

## Related

- [[projects/echooo-agent-gogetajob]]
- [[projects/echooo-agent-flowforge]]
- [[experiments/exp-001-first-workloop]]
