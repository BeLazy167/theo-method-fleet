# Snapshots

Generated. Do not hand-edit anything in here.

`bin/render` renders the sources in `corefiles/` and `skills/` into one directory per harness. The snapshot shows the fleet-managed files a harness should receive.

```
snapshots/
  claude/   AGENTS.md, CLAUDE.md, skills/<name>/SKILL.md
  codex/    AGENTS.md, skills/<name>/SKILL.md
```

Two reasons this exists instead of syncing `skills/` directly:

1. **Tiers flatten here, not on the target.** A machine sees one flat `skills/` directory. `rsync --delete` is safe only inside that managed subtree, never against the whole harness home.
2. **You can read what a harness will get.** Diff a snapshot against the last one before you apply, and you know exactly what changes on every box.

Snapshots are generated and gitignored. Review the source diff, then rerun `bin/render` before installing.
