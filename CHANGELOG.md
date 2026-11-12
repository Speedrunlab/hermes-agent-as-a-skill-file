# Changelog

Hermes Agent as a Skill File. Newest first.

## v2.1.1 (12 Nov 2026)

Housekeeping release. Added this changelog, with earlier entries backfilled from the git history and the README's "What's new" sections. Bumped the version to 2.1.1. Added "Not affiliated." to the inspiration credit in the README and SKILL.md. No behaviour change.

## v2.1.0 (23 Oct 2026)

Checked line by line against the Hermes Agent source and documentation at commit 687e00e0 (174 commits after v2.0.0's review; the learning-loop code itself changed only in two refactors, so v2.0.0 was still current). Added what the skill had not yet covered, in its own words: saving during the task, not only in the review; the review's tool whitelist, auto-deny, iteration and input caps, the digest on a cheaper model, the idle queue on shared compute, and the on-demand review with a focus; learning a skill from a source, including the knowledge-base layout for books and specs (references/learning-from-sources.md); installing skills from outside: scanning, trust levels, provenance, repository skills, schedules as proposals, sharing between agents (references/installing-and-sharing-skills.md); curator details (first-run wait, dry run, reports, adoption, restore, explicit purge), the save-time scan of entries, naming the agent's own tools in a skill, proposing an automation for a recurring ask, and an inspectable list of everything learned.

Not carried over, on purpose: anything that is the runtime rather than the loop.

## v2.0.0 (22 Oct 2026)

Rebuilt against the Hermes Agent source at commit 81a55437. v1 described the April 2026 Hermes; since then Hermes has moved learning into a background review that runs after the reply; adopted "skills first, memory last" and declarative memory (facts, not orders); defined the skill shape; added a never-capture list and read-before-write for skill patches; added a curator and an audit ledger with rollback; and added write_approval gates for memory and skills.

v2 is also agent-agnostic: runtime-specific paths and hook names have moved out of SKILL.md into the install notes. The skill's name now matches the repo and the install command (hermes-agent-as-a-skill-file; v1 was self-improving-agent).

## v1 (21 Apr 2026)

Initial release of Hermes Agent as a skill file: a self-improvement loop for existing agents. v1 described the April 2026 Hermes. The skill's name then was self-improving-agent.
