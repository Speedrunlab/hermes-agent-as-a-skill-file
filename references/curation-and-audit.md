# Curation and audit: the operational detail

Depth for SKILL.md sections 7 and 8. The always-on rules (lifecycle thresholds, protections, jurisdiction, the ledger line per mutation) stay in `SKILL.md`; this file holds how a run is triggered and reported, how adoption, restore and purge work, the ledger's shape, versioning, metrics and the learning timeline.

## The curator's run

- **Trigger:** an inactivity check, not a scheduled job: at session start, or on a maintenance timer, run when the last run is older than the cadence (about 7 days) **and** the agent has been idle for at least 2 hours. Never while a turn is running. Several processes serving one profile can race the check; one owner (a gateway, when there is one) runs the chores.
- **First enablement:** seed "last run" to now and wait one full cadence before the first real pass. A fresh install or an update never mutates the library on its first tick; the operator has the interval to pin, adopt or opt out.
- **Dry run:** produce the same report without changing anything. Offer it as the way to preview a pass.
- **Two phases.** Deterministic transitions (always on, no model call): unused for the stale threshold becomes `stale`, unused for the archive threshold is moved to `.archive/`. Consolidation (opt-in, a model pass with a high iteration ceiling; a full sweep can take 50 to 100 calls): the pass surveys only skills it can both read and write, decides per skill to keep, patch, consolidate into a class-level skill or archive with an "absorbed into" note, and treats a skill as a package: keep it standalone, re-home its support files with their paths rewritten, or archive the whole thing; never flatten a skill into another's `references/`.
- **Per-run report:** every transition with its reason, what the consolidation pass changed and said, and a rename map (old name to new name) whenever skills were merged, so a wave of renames is visible at a glance.
- **Telemetry per skill:** times used (loaded into a prompt), times viewed (read in full), times patched, the time of each, created at, state (`active`, `stale`, `archived`), pinned, archived at. Bundled and installed skills are excluded from telemetry writes.

## Jurisdiction, adoption, pinning

- **Agent-created** means the review pass wrote it; only that path sets the marker. A skill the foreground turn wrote on the operator's request is the operator's (marked as learned on request); hand-written, bundled, installed and repository skills are never curated.
- **Adoption** hands an unmanaged skill to the curator by the operator's declaration. It never resets the skill's inactivity clock, so a long-idle library handed over goes stale on the next pass, which is the point. Authorship is never inferred from telemetry: many patches prove the agent maintains a skill, not that it wrote it. Adoption refuses bundled, installed, external and protected skills.
- **Pinned skills** skip every automatic transition and the consolidation pass, and the agent's own delete is refused on them (patches still go through, so a pinned skill keeps improving). A skill any scheduled job references, paused or not, is protected from transitions the same way. The marker is stored with the skill's telemetry so it survives sessions.
- **Restore** moves an archived skill back and resets its state to active; refuse when a bundled or installed skill of the same name has since arrived, since the restored copy would shadow it.
- **Purge** deletes archives older than a retention the operator sets (default: never). It runs only on the operator's explicit command, offers a dry run, and captures every purged skill into the ledger first, so even a purge leaves a recoverable trail.

## Backups and rollback

- Before a consolidation pass (the only pass that rewrites skill content in place), snapshot the live skills tree. The archive, the ledger and the hub state are never rolled in, and a rollback never rewinds them (an older copy would lose archived skills or ledger entries).
- Keep a small number of snapshots (two) and prune older ones. A rollback takes a "pre-rollback" snapshot first, so a mistaken rollback can itself be undone. Manual snapshots carry a reason.
- The prune-only pass takes no snapshot: moving whole directories into `.archive/` is its own undo.

## The ledger

- One entry per mutation by any actor: the curator's transitions, the agent's own skill writes, the operator's commands. Fields: actor (`curator`, `agent`, `operator`), action (create, edit, patch, delete, write_file, remove_file, archive, restore, purge, rollback), evidence (the "absorbed into" target of a consolidation, the triggering session), and before/after manifests of each file's path and content hash, with contents stored once per hash.
- **Single-entry rollback** restores exactly the files that one mutation touched (and removes files it created), takes a safety entry of the current state first, and fails closed: if the safety capture cannot be written, nothing moves. Because foreground deletes are ledgered too, a hard-deleted skill can be resurrected.
- The ledger is telemetry, never a gate: a failed ledger write does not block the mutation. It is size-bounded; when it grows past the cap, compact through the unchanged-file dedup, then drop the oldest entries, never the newest. The operator can set the cap to "never".

## Versioning and deprecation

Semantic versions in frontmatter. Patch = wording; minor = a new step, branch or pitfall; major = a changed or removed procedure. Retire with `deprecated: true` and `superseded_by:`, then archive.

## Metrics (monthly, or on request)

Skills created, patched, archived and rolled back; reviews that said "Nothing to save"; recall hits against times a user had to repeat themselves; repeated errors after a lesson was recorded (should trend to zero); memory use per store as a share of budget; the review pass's cost per session (calls and tokens, logged per pass); curator runs and their transitions.

## The learning timeline

Everything learned, listed oldest to newest: saved skills and memory entries, each with its date. A skill appears once it has a learning signal (created by the review pass, learned on request, or used at least once); bundled and hand-written skills never used stay out. The operator can open any item and edit it in place, or remove it: a removed skill is archived (restorable), a removed memory entry is deleted. Pass an item back by the exact identifier the list printed.
