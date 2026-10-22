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

Installs Hermes Agent's closed learning loop into any agent. The skill is **agent-agnostic**: it describes behaviour, never a specific tool, hook or path, and a one-page table maps each capability to whatever your agent provides.

- **A review pass after every task.** Once the answer is delivered, the agent reviews the conversation and updates its skills and memory (memory every 10 user turns, skills every 15 tool iterations, plus every session boundary and compaction). It never competes with the work.
- **Skills first, memory last.** What the agent learns while doing a task goes into that task's skill. Memory holds only the few facts every session needs, in two budgeted stores (user profile and agent notes).
- **Class-level skills, written as lessons, not logs.** Procedure first, one rule per lesson with the reason, no ticket numbers or diaries, fixed in place when wrong.
- **A never-capture list.** Environment failures, "tool X is broken" claims, transient errors, one-off tasks and unresolved dead ends never become rules.
- **Recall before asking.** Search past sessions before making anyone repeat themselves.
- **A user model that sharpens.** Corrections and preferences land in the skill that governs that work.
- **Approval gates and curation.** New skills and skill patches are staged for the operator by default; unused skills go stale after 14 days and are archived (never deleted) after 30.
- **Audit and rollback.** A ledger of every write, prior versions kept, monthly metrics, repeat errors escalated.

---

## What's new in v2.0.0 (22 Oct 2026)

Rebuilt against the Hermes Agent source at commit `81a55437`. v1 described the April 2026 Hermes; since then Hermes has:

- moved learning into a **background review** that runs after the reply, with its own prompts (now adapted into section 1);
- adopted **"skills first, memory last"** and **declarative memory** (facts, not orders);
- defined the **skill shape**: class-level umbrellas, `references/` · `templates/` · `scripts/`, lessons not logs;
- added a **never-capture list** and **read-before-write** for skill patches;
- added a **curator** (stale → archived, pins, never delete) and an **audit ledger** with rollback;
- added `write_approval` gates for memory and skills.

v2 is also **agent-agnostic**: runtime-specific paths and hook names have moved out of `SKILL.md` into the install notes below.

The skill's name now matches the repo and the install command (`hermes-agent-as-a-skill-file`; v1 was `self-improving-agent`).

---

## Install

Put the folder in your agent's skills directory, then fill in the **Map it to your agent** table at the end of `SKILL.md` once. Notes for common runtimes:

### Claude Code
```bash
mkdir -p ~/.claude/skills
git clone https://github.com/Speedrunlab/hermes-agent-as-a-skill-file.git ~/.claude/skills/hermes-agent-as-a-skill-file
```
Then run: `/hermes-agent-as-a-skill-file`. Review pass: a background subagent or the end of the turn. Session boundary and compaction: the `Stop` and `PreCompact` hooks. Project-scoped install: `.claude/skills/hermes-agent-as-a-skill-file/`.

### OpenClaw
```bash
mkdir -p ~/.openclaw/skills
git clone https://github.com/Speedrunlab/hermes-agent-as-a-skill-file.git ~/.openclaw/skills/hermes-agent-as-a-skill-file
```
OpenClaw picks it up automatically on next session. Use the daily log `memory/YYYY-MM-DD.md` as the audit log.

### NemoClaw
Place in `.agents/skills/hermes-agent-as-a-skill-file/SKILL.md` within your NemoClaw directory.

### Hermes Agent
No install needed: Hermes runs this loop natively. The matching config keys are `memory.nudge_interval` and `skills.creation_nudge_interval` (review triggers), `memory.write_approval` and `skills.write_approval` (approval gates), the `curator:` block (curation), and `auxiliary.background_review` (run the review on a cheaper model).

### Any AgentSkills-compatible framework
Copy `SKILL.md` into your agent's skills directory, or load its contents into your agent's system prompt directly.

---

## Compatibility

| Platform | Supported |
|----------|-----------|
| Claude Code | ✅ |
| OpenClaw | ✅ |
| NemoClaw | ✅ |
| Hermes Agent | Built in (config keys above) |
| Any AgentSkills-compatible framework | ✅ |
| Any LLM with system prompt access | ✅ (manual load) |

---

## Required capabilities

Before installing, verify your platform provides:

| Capability | Required? |
|------------|-----------|
| Persistent memory store with a size budget | Yes |
| Search over past sessions or logs | Yes |
| Writable skills directory | Yes |
| User-facing message channel | Yes |
| A way to run work after the reply (post-turn hook or background subagent) | Recommended |
| Pre-compaction or session-end hook | Recommended |
| Scheduler or heartbeat | Recommended |
| Daily log or persistent audit store | Recommended |

If a recommended capability is missing, that trigger falls back to the end of the session. Nothing breaks.

---

## Credits

Based on [Hermes Agent](https://github.com/nousresearch/hermes-agent) by Nous Research (MIT licence), reviewed at commit `81a55437`. The review prompts, skill-shape rules and never-capture list are adapted from its source.

Adapted and packaged as a portable skill file by [Speedrun AI Labs](https://www.speedrunlab.ai).

**Not an official Nous Research artifact.** This is a derivative work based on the Hermes Agent source and documentation. Attribution, not affiliation.

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
