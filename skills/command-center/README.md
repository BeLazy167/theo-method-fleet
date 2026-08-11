# Command-center skills

Skills that belong only on the leader machine — the box you drive the fleet from.
They operate *on* the fleet rather than *within* a project, so putting them on the
other boxes is pointless at best and dangerous at worst.

Installs into `~/.claude/skills/` and `~/.codex/skills/` on the leader only. Never
synced outward. `metadata.scope` is `command-center`.

Current members:

- `ccusage-fleet` — aggregates Claude and Codex usage across local and SSH-connected boxes.
- `provision-box` — brings a new machine up to fleet standard. Runs from the machine doing the provisioning, not the one being provisioned.

If a skill here stops being leader-specific, move it to `universal/` and update `metadata.scope`.
