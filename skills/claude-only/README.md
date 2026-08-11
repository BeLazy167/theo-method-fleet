# Claude-only skills

Skills that depend on something specific to Claude Code — its transcript format, its tool names, its session layout. They would misfire or read the wrong files under another harness.

Installs into `~/.claude/skills/` only. `metadata.harness` is `[claude]`.

Current members: none. The video shows this tier, but no global Claude-only skill is disclosed. Agent-history analysis lives in `scratch-log-audit/` as repository working material rather than an installed skill.

If a skill here stops depending on Claude specifics, widen `metadata.harness` and move it to `universal/`.
