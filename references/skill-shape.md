# Skill shape: frontmatter, body order, support files

Depth for SKILL.md section 2. Read it when creating a new skill (the last resort in "Where a lesson goes") or restructuring one.

## Frontmatter

```
---
name: lowercase-hyphenated-class-name     # at most 64 characters, no spaces
description: "One sentence, at most 60 characters, the capability."
version: 0.1.0
author: <your organisation>                # a fixed value, never the OS username or git identity
platforms: [macos, linux]                  # only when the skill uses OS-bound commands
---
```

- **Description, at most 60 characters.** Some skill indexes truncate the description there and load it every session, so anything past the cut never routes. State the capability, not the implementation; no marketing words; do not repeat the name. Count the characters after writing it; if it is over, cut it before saving. A description that contains a colon is wrapped in double quotes.
- **Author is a fixed value.** Never fill it from the host environment (the login name, git identity, anything probed): skills get shared and published, and an environment-derived name is a privacy leak the user never opted into.
- **Platforms only when bound.** `osascript`, `apt`, `systemctl` bind to an OS; `/proc`, `os.setsid`, `SIGKILL` to Linux; `fcntl`, `termios` to POSIX. Prefer the cross-platform form first (a temp-dir API, a path library, Python filtering instead of a shell filter) and gate only when the dependency is genuinely platform-bound. Omit the field for a portable skill.
- **Retiring a skill:** `deprecated: true` and `superseded_by: <name>` (this skill's convention), then archive.

## Body order

`# Title`, then a 2-3 sentence intro (what it does, what it does not, the dependency stance), then:

1. `## When to Use`: concrete trigger phrases.
2. `## Prerequisites`: exact environment variables, install steps, credentials, by name only (a secret's value never appears in a skill).
3. `## How to Run`: the canonical invocation, framed through the agent's own tools.
4. `## Quick Reference`: a flat command or endpoint list, no narration.
5. `## Procedure`: numbered steps with copy-exact commands; pitfalls attached to the step they affect.
6. `## Pitfalls`: known limits, rate limits, things that look broken but are not.
7. `## Verification`: one command or check that proves the skill worked.

Omit a section only when it has no content. Put the most common workflow first; edge cases go last.

## Size

About 100 lines for a simple skill, 200 for a complex one. A `SKILL.md` body over about 24,000 characters is too large: it loads whole and stays in context for the session. More than about 60 reference files means the skill is hoarding. Before saving, check the three failure shapes: an incident log (a body dense in ticket or PR numbers), reference sprawl, an oversized body. For a knowledge-base skill the body cap applies to `SKILL.md` itself; the distilled content lives in `references/` (see `learning-from-sources.md`).

## Support files

- `references/<topic>.md`: depth needed only sometimes (a decision table, a recipe, provider quirks, condensed domain notes). Named by topic and extended in place; never one file per session or per incident, never a pasted error transcript.
- `templates/<name>.<ext>`: starter files meant to be copied and modified (boilerplate configs, scaffolding, a known-good example).
- `scripts/<name>.<ext>`: actions the agent should run rather than retype (checks, probes, fixture generators, parsers). Do not make the model inline-write a parser every call.

Every new support file gets a one-line pointer in `SKILL.md` so a future session knows it exists. Reference a bundled script by a skill-relative path.
