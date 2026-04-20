# hermes-agent-as-a-skill-file

**Hermes Agent as a skill file: Upgrade your existing agent, no need to migrate.**

Free and open source · MIT Licence · Built by [Speedrun AI Labs](https://www.speedrunlab.ai), Sydney, Australia · Inspired by [Nous Research](https://github.com/nousresearch/hermes-agent) (MIT)

---

## The problem

Nous Research released Hermes Agent — a genuinely useful learning-loop architecture for autonomous agents. Auto skill creation, cross-session recall, memory capacity management, periodic self-checks, compound learning. Real capabilities that make agents meaningfully better over time.

The problem: getting those capabilities means migrating to the Hermes Agent runtime. For teams already running OpenClaw, NemoClaw, or any other AgentSkills-compatible framework, that is a hard sell.

This skill packages the Hermes Agent learning-loop patterns as a portable SKILL.md file. Drop it into your existing agent. No migration. No new runtime. Same agent, upgraded behaviour.

---

## What it does

Installs a closed self-improvement loop into any compatible agent:

- **Auto skill creation** — after complex tasks, the agent drafts a reusable skill for operator review
- **Periodic self-checks** — every 15 tool calls, pause and evaluate what's worth persisting
- **Skill refinement** — when a skill is outdated, propose a targeted patch; operator approves before it applies
- **Memory flush before context loss** — persists what matters before compaction or session reset
- **Memory capacity management** — tiered thresholds (80% / 90% / 95%) with consolidation rules
- **Cross-session recall** — search history before asking anyone to repeat themselves
- **Active user model refinement** — preferences and corrections build up over time
- **Compound learning** — every mistake becomes a structured lesson; repeated errors escalate
- **Skill versioning and lifecycle** — semantic versioning, deprecation, rollback support
- **Observability** — monthly metrics: skills proposed, approved, reverted, recall hit rate
- **Governance hierarchy** — explicit conflict resolution order when skills, memory, and lessons disagree
- **Inter-agent skill sharing** — export/import approved skills across agents with operator-gated activation

---

## Install

### Claude Code
```bash
mkdir -p ~/.claude/skills
git clone https://github.com/Speedrunlab/hermes-agent-as-a-skill-file.git ~/.claude/skills/hermes-agent-as-a-skill-file
```
Then run: `/hermes-agent-as-a-skill-file`

### OpenClaw
```bash
mkdir -p ~/.openclaw/skills
git clone https://github.com/Speedrunlab/hermes-agent-as-a-skill-file.git ~/.openclaw/skills/hermes-agent-as-a-skill-file
```
OpenClaw picks it up automatically on next session.

### NemoClaw
Place in `.agents/skills/hermes-agent-as-a-skill-file/SKILL.md` within your NemoClaw directory.

### Hermes Agent
```bash
~/.hermes/skills/hermes-agent-as-a-skill-file/SKILL.md
```

### Any AgentSkills-compatible framework
Copy `SKILL.md` into your agent's skills directory, or load its contents into your agent's system prompt directly.

---

## Compatibility

| Platform | Supported |
|----------|-----------|
| Claude Code | ✅ |
| OpenClaw | ✅ |
| NemoClaw | ✅ |
| Hermes Agent | ✅ |
| Any AgentSkills-compatible framework | ✅ |
| Any LLM with system prompt access | ✅ (manual load) |

---

## Required capabilities

Before installing, verify your platform provides:

| Capability | Required? |
|------------|-----------|
| Persistent memory write | Yes |
| Memory or history search | Yes |
| Writable file system | Yes |
| User-facing message channel | Yes |
| Pre-reset or pre-compaction hook | Recommended |
| Scheduler or heartbeat | Recommended |
| Daily log or persistent audit store | Recommended |

If a recommended capability is missing, the relevant section degrades gracefully to manual triggers. Nothing breaks.

---

## Credits

Inspired by the Hermes Agent learning-loop patterns by [Nous Research](https://github.com/nousresearch/hermes-agent) (MIT licence).

Adapted and packaged as a portable skill file by [Speedrun AI Labs](https://www.speedrunlab.ai).

**Not an official Nous Research artifact.** This is a derivative work inspired by publicly documented Hermes Agent design patterns. Attribution, not affiliation.

---

## Part of the Speedrun AI Labs Skill Library

This skill is part of the open-source skill library at [speedrunlab.ai](https://www.speedrunlab.ai).

Other skills:
- [/deep-audit](https://github.com/Speedrunlab/deep-audit) — four-phase code auditor with evidence standards
- [/no-slop-writing](https://github.com/Speedrunlab/no-slop-writing) — eliminates AI-speak from generated text (American English)
- [/no-slop-writing-au](https://github.com/Speedrunlab/no-slop-writing-au) — same, Australian English edition
- [/read-before-write](https://github.com/Speedrunlab/read-before-write) — forces any AI agent to read files before editing them

---

## Licence

MIT. Free to use, modify, and distribute.

---

*Built in Sydney by Speedrun AI Labs.*
