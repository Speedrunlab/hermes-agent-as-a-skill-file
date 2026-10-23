# Map it to your agent

Read once, at install. `SKILL.md` names capabilities, never a particular tool, hook or path; this table says what to look for in your agent and what to do when it has none. Record the mapping in the agent's own notes or config, not in the skill, so the skill stays portable.

| This skill says | Look for in your agent | If there is none |
|---|---|---|
| Memory store (user profile + agent notes) | A persistent memory tool, memory files, or a key-value store loaded at session start | Two small text files read at session start |
| Skills directory | Wherever the agent discovers skills or instructions | A folder whose files you load on demand |
| Session search | Full-text or semantic search over past conversations or logs | Search the daily log or audit log |
| Review pass | A post-turn hook, a background subagent, or a forked context | The last step of the turn |
| Session boundary, compaction | Session-end, reset and pre-compaction hooks | Review at the end of every session |
| Review on demand | A "refine" or "review" command | "Review this session" said in chat |
| Learning from a source | A "learn" command | The ask in chat, with `references/learning-from-sources.md` |
| Staging and approval | An approval queue | `<skills_root>/_staging/` plus a one-line message to the operator |
| Installing an outside skill | A hub installer with a scanner and a provenance record | Read the whole bundle, scan by hand, record source and hash in the ledger |
| Scheduler (curation) | Cron, heartbeat, or a check at session start | Curate when the operator asks |
| Audit log | An append-only log or daily notes | `<skills_root>/_ledger.jsonl` |

Notes for common runtimes are in the repository README (Claude Code, OpenClaw, NemoClaw, Hermes Agent itself, any AgentSkills-compatible framework).
