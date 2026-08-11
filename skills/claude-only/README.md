# Claude-only skills

Skills that depend on something specific to Claude Code — its transcript format, its tool names, its session layout. They would misfire or read the wrong files under another harness.

Installs into `~/.claude/skills/` only. `metadata.harness` is `[claude]`.

Current members:

- `audit-agent-history` — reads past session transcripts to find what your agents keep getting wrong, before you write a rule about it.

If a skill here stops depending on Claude specifics, widen `metadata.harness` and move it to `universal/`.
