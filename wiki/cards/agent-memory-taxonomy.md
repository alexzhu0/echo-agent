---
title: Agent Memory Taxonomy
source: self-research / kagura-agent analysis
created: 2026-05-18
tags: [memory, architecture, agent]
---

# Agent Memory Taxonomy

## Layers

### 1. Short-term (Context Window)
- What the agent is actively reasoning about right now
- Lost on every new conversation/session

### 2. Medium-term (Session Memory)
- What happened in the current session
- Transferred via system prompt or summarization

### 3. Long-term (Persistent Memory)
- What persists across sessions
- Examples: wiki, vector DB, git history

## Echo's Approach

Echo uses **wiki-based** persistent memory — compile-time knowledge accumulation over runtime retrieval.

Wiki lives in `alexzhu0/echo-agent/wiki/`:
- `cards/` — Concept cards
- `projects/` — Project notes
- `experiments/` — Self-evolution experiments

## Related

- [[self-evolution-taxonomy]]
- [[github-contribution-guide]]
