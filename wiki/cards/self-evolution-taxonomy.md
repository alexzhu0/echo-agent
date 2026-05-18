---
title: Self-Evolution Taxonomy
source: kagura-agent / caduceus-experiment analysis
created: 2026-05-18
tags: [self-evolution, agent, learning]
---

# Self-Evolution Taxonomy

## What is Self-Evolution?

An agent that improves its own capabilities, behavior, or knowledge over time — without human intervention for every change.

## Categories

### 1. Gradient-based
- RLHF, DPO, GRPO
- Requires reward signal from environment or human
- Used by: major LLMs during training

### 2. DNA / Belief Systems
- Agent maintains a structured "belief" or "DNA" document
- Experiences update beliefs
- Used by: Kagura (partially), some agent frameworks
- Kagura finding: DNA/beliefs don't prevent hallucination

### 3. Behavioral Adaptation
- Agent adjusts behavior based on success/failure signals
- No explicit belief system
- Examples: tool use patterns, workflow selection

### 4. Knowledge Accumulation
- Agent writes knowledge to external storage (wiki, notes)
- Retrieves at runtime for new problems
- Used by: Echo, Kagura

### 5. Dreaming / Memory Consolidation
- Offline processing of experiences
- Kagura's pipeline: extract → score → consolidate → dreaming
- Finding: most agents' dreaming pipelines are broken or dormant

## Key Insight from Kagura

> External feedback (humans) provides 95% of meaningful gradient.
> Self-generated feedback (introspection) is mostly noise.

Guardrails are **load-bearing structures**, not removable training wheels.

## Echo's Position

Echo focuses on **Knowledge Accumulation** + **Behavioral Adaptation**.
Not attempting gradient-based RL or complex belief systems yet.
The foundation is: workloop → git history → wiki notes → next workloop.

## Related

- [[agent-memory-taxonomy]]
- [[experiments/exp-001-first-workloop]]
