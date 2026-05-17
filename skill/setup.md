# Installing the pr-cold-state Skill

This skill turns the methodology in `methodology/pr-contribution.md` into
something a runtime agent (Hermes Agent, Claude Code, or any SKILL.md-aware
agent) loads automatically when about to do OSS contribution work.

## Hermes Agent

```bash
# One-time: link the skill into Hermes's skill directory
ln -s "$(pwd)/skill" ~/.hermes/skills/pr-cold-state
# Or copy if you prefer:
# cp -r skill ~/.hermes/skills/pr-cold-state

# Verify discovery
hermes tools | grep pr-cold-state

# Invoke explicitly
hermes
> /pr-cold-state
# Or let auto-activation handle it next time you say:
> open a PR to upstream NousResearch/hermes-agent
```

## Claude Code

```bash
# Claude Code looks at ~/.claude/skills/
mkdir -p ~/.claude/skills
ln -s "$(pwd)/skill" ~/.claude/skills/pr-cold-state

# Restart Claude Code session. Skill activates by description match.
```

## What activates it

The skill's `description` field is the trigger. It activates when the
agent identifies the current task involves any of:
- "open a PR" / "create pull request" / "gh pr create"
- "contribute to <upstream>"
- "evaluate this issue" / "should I fix this bug"
- "submit PR" / "comment on competing PRs"
- "scan for OSS issues"
- "rebase my open PRs"

When activated, the agent loads `SKILL.md` into context. The agent then
walks the 6-condition SUBMIT GATE before any code/comment goes out.

## Configuration

The skill declares two config keys in its frontmatter:

```yaml
metadata:
  hermes:
    config:
      - key: pr_cold_state.upstream
        description: Target upstream repo slug (e.g., "NousResearch/hermes-agent")
      - key: pr_cold_state.identity
        description: GitHub handle the agent is contributing as
      - key: pr_cold_state.relationship_state
        description: cold | warming | established
        default: cold
```

Set these in `~/.hermes/config.yaml` (or your runtime's equivalent):

```yaml
pr_cold_state:
  upstream: NousResearch/hermes-agent
  identity: alexzhu0
  relationship_state: cold
```

Per-upstream overrides recommended once you contribute to more than one
repo — relationship state is per (agent, repo) pair, not global.

## What this skill does NOT do (yet)

This is the **advisory** layer. It loads context, it does not enforce.
The agent reads it but is technically free to ignore it.

To make the GATE **enforceable** (block PR creation if gate score < 5/6),
the next step is wiring it into the work engine. For Echo's stack:
- `gogetajob submit` should call out to a gate-checker script that
  consults this skill's rules
- If gate score < 5: block submit, report which condition failed

That's the v0.3 work. This v0.2 ships the advisory layer first because:
1. Even advisory loading changes agent behavior measurably
2. Enforcement requires a stable advisory layer to enforce against
3. Skill-only is reversible; enforcement creates coupling

## Verifying it works

After install, the simplest test is asking the agent to plan an OSS contribution:

```
You: I want to fix a bug in NousResearch/hermes-agent. Issue #26344
     describes a credential_pool gap. Should I open a PR?

Agent: [if skill loaded] First, let me run the SUBMIT GATE for #26344...
       [if skill NOT loaded] Sure, here's a draft fix...
```

If the agent jumps to drafting code without running the gate, the skill
didn't activate — check description matching and skill installation path.

## Updating the skill

When new lessons are added to `wiki/lessons/L*.md` or the methodology
revises, the skill SKILL.md's pointer table should be updated to include
them. The skill body itself stays compact; details live in the linked files.

The agent loads SKILL.md every activation; it loads referenced files only
when it needs them. Keeps prompt budget tight.
