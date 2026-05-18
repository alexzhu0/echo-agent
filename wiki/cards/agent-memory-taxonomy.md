---
title: Agent Memory Taxonomy
source: kagura-agent wiki / self-research
created: 2026-05-18
tags: [memory, architecture, agent]
---

# Agent Memory Taxonomy

## Layers

### 1. Short-term (Context Window)
- What the agent is actively reasoning about right now
- Lost on every new conversation/session
- Examples: working memory, attention context

### 2. Medium-term (Session Memory)
- What happened in the current session
- Transferred via system prompt updates or summarization
- Examples: conversation summary, session state

### 3. Long-term (Persistent Memory)
- What persists across sessions
- Examples: wiki, vector DB, git history

## Long-term Memory Strategies

### Wiki-based (Compile-time)
- Agent writes knowledge to markdown files
- Human-curated or agent-generated
- Compile-time retrieval: agent reads files when relevant
- Used by: Kagura, Echo
- Pros: inspectable, version-controlled, human-readable
- Cons: requires discipline to maintain

### Vector DB (Runtime)
- Embed and store knowledge chunks
- Retrieve at runtime via similarity search
- Pros: automatic, scales well
- Cons: black box, hard to inspect or edit

### Hybrid
- Wiki for structured, high-value knowledge
- Vector DB for broad recall
- Used by: production AI assistants

## Echo's Approach

Echo uses **wiki-based** persistent memory.

- Wiki lives in `alexzhu0/echo-agent/wiki/`
- Concept cards in `cards/`
- Project notes in `projects/`
- Stories in `stories/`

Wiki is Echo's externalized cognition — not a database, but a second brain made of markdown.

## Related

- [[cards/self-evolution-taxonomy]]
- [[github-contribution-guide]]
