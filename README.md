# hermes-agent-as-a-skill-file

**Hermes Agent as a skill file: Upgrade your existing agent, no need to migrate.**

Free and open source · MIT Licence · Built by [Speedrun AI Labs](https://www.speedrunlab.ai), Sydney, Australia · Inspired by [Nous Research](https://github.com/nousresearch/hermes-agent) (MIT)

---

## The problem

Nous Research released Hermes Agent, a genuinely useful learning-loop architecture for autonomous agents. Auto skill creation, cross-session recall, memory capacity management, periodic self-checks, compound learning. Real capabilities that make agents meaningfully better over time.

The problem: getting those capabilities means migrating to the Hermes Agent runtime. For teams already running OpenClaw, NemoClaw, or any other AgentSkills-compatible framework, that is a hard sell.

This skill packages the Hermes Agent learning-loop patterns as a portable SKILL.md file. Drop it into your existing agent. No migration. No new runtime. Same agent, upgraded behaviour.

---

## What it does

Installs Hermes Agent's closed learning loop into any agent. The skill is **agent-agnostic**: it describes behaviour, never a specific tool, hook or path, and a one-page table maps each capability to whatever your agent provides.

- **Saving during the task, and a review pass after it.** A non-trivial workflow, a skill that proved wrong, or "remember this" is saved as it happens. Once the answer is delivered, the agent also reviews the conversation and updates its skills and memory (memory every 10 user turns, skills every 10 to 15 tool iterations, plus every session boundary and compaction), on a capped budget, with only the skill, memory and read-only file tools, and on request with a focus. It never competes with the work.
- **Skills first, memory last.** What the agent learns while doing a task goes into that task's skill. Memory holds only the few facts every session needs, in two budgeted stores (user profile and agent notes).
- **Class-level skills, written as lessons, not logs.** Procedure first, one rule per lesson with the reason, no ticket numbers or diaries, fixed in place when wrong.
- **A never-capture list.** Environment failures, "tool X is broken" claims, transient errors, one-off tasks and unresolved dead ends never become rules.
- **Recall before asking.** Search past sessions before making anyone repeat themselves.
- **A user model that sharpens.** Corrections and preferences land in the skill that governs that work.
- **Learning a skill from a source on request.** A code directory, a documentation page, pasted notes, the workflow just completed, or a whole book or spec: the request is the spec, an existing skill is extended before a new one is created, and a large source becomes a knowledge-base skill (a lean index plus one distilled reference per chapter) rather than a lossy summary or a copy.
- **Approval gates and curation.** New skills and skill patches are staged for the operator by default; unused skills go stale after 14 days and are archived (never deleted) after 30. The curator waits one cadence on first enablement, supports a dry run, writes a report per run, and purges archives only on the operator's explicit command.
- **Skills from outside are scanned, never self-installed.** Exfiltration, prompt injection, destructive commands, supply-chain signals, invisible characters and anything that would execute on load; provenance recorded so updates never overwrite local edits; a repository's own skills need the operator's trust once and are re-scanned on change.
- **Audit and rollback.** A ledger of every write, prior versions kept, monthly metrics, repeat errors escalated, and everything learned listable and editable by the operator.

---

## What's new in v2.1.0 (23 Oct 2026)

Checked line by line against the Hermes Agent source and documentation at commit `687e00e0` (174 commits after v2.0.0's review; the learning-loop code itself changed only in two refactors, so v2.0.0 was still current). Added what the skill had not yet covered, in its own words:

- saving during the task, not only in the review; the review's tool whitelist, auto-deny, iteration and input caps, the digest on a cheaper model, the idle queue on shared compute, and the on-demand review with a focus;
- learning a skill from a source, including the knowledge-base layout for books and specs (`references/learning-from-sources.md`);
- installing skills from outside: scanning, trust levels, provenance, repository skills, schedules as proposals, sharing between agents (`references/installing-and-sharing-skills.md`);
- curator details (first-run wait, dry run, reports, adoption, restore, explicit purge), the save-time scan of entries, naming the agent's own tools in a skill, proposing an automation for a recurring ask, and an inspectable list of everything learned.

Not carried over, on purpose: anything that is the runtime rather than the loop (the terminal, browser and messaging surfaces, goals, loops, heartbeats, kanban, cron delivery, the hub's registries, the model and provider plumbing).

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

Put the folder in your agent's skills directory, then map each capability once with the table in `references/map-to-your-agent.md`. Notes for common runtimes:

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
No install needed: Hermes runs this loop natively. The matching config keys are `memory.nudge_interval` and `skills.creation_nudge_interval` (review triggers), `memory.write_approval` and `skills.write_approval` (approval gates), the `curator:` block (curation), `auxiliary.background_review` (a cheaper model, `max_input_tokens`, `extra_tools`, `defer`, `enabled`), and `skills.ledger`. The commands are `/refine [focus]` (review on demand), `/learn <sources and requirements>` (learn a skill from a source), `/journey` (everything learned, editable), `/skills pending|diff|approve|reject` and `/memory pending|approve|reject` (the gates), `hermes curator` (status, dry run, pin, adopt, restore, purge, ledger, rollback) and `hermes skills install|check|update|trust` (the hub).

### Any AgentSkills-compatible framework
Copy the whole folder into your agent's skills directory, or load `SKILL.md` into your agent's system prompt directly and keep `references/` where the agent can read it.

---

## What's in the box

```
SKILL.md                                    the loop: review, skills, memory, recall, curation, audit, governance
references/
  map-to-your-agent.md                      the install-time mapping of each capability to your agent
  skill-shape.md                            frontmatter, body order, size, support files
  learning-from-sources.md                  learning a skill from material; the knowledge-base layout
  installing-and-sharing-skills.md          scanning, trust, provenance, repository skills, sharing between agents
  curation-and-audit.md                     the curator's run, adoption, backups, the ledger, metrics, the learning timeline
```

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

Based on [Hermes Agent](https://github.com/nousresearch/hermes-agent) by Nous Research (MIT licence), reviewed at commit `687e00e0`. The review prompts, skill-shape rules, never-capture list and learn-from-source standards are adapted from its source.

Adapted and packaged as a portable skill file by [Speedrun AI Labs](https://www.speedrunlab.ai).

**Not an official Nous Research artifact.** This is a derivative work based on the Hermes Agent source and documentation. Attribution, not affiliation.

---

## Part of the Speedrun AI Labs Skill Library

This skill is part of the open-source skill library at [speedrunlab.ai](https://www.speedrunlab.ai).

Other skills:
- [/reverse-engineer-anything-self-learning-loop](https://github.com/Speedrunlab/reverse-engineer-anything-self-learning-loop), examine a product you are authorised to rebuild, build your own, and improve it against the original until it is 100% there
- [/reverse-engineer-anything](https://github.com/Speedrunlab/reverse-engineer-anything), authorised analysis of shipped software with standard open-source tools
- [/deep-audit](https://github.com/Speedrunlab/deep-audit), four-phase code auditor with evidence standards
- [/no-slop-writing](https://github.com/Speedrunlab/no-slop-writing), eliminates AI-speak from generated text (American English)
- [/no-slop-writing-au](https://github.com/Speedrunlab/no-slop-writing-au), same, Australian English edition
- [/read-before-write](https://github.com/Speedrunlab/read-before-write), forces any AI agent to read files before editing them

---

## Licence

MIT. Free to use, modify, and distribute.

---

*Built in Sydney by Speedrun AI Labs.*
