# Snapshots

Generated. Do not hand-edit anything in here.

`apply-fleet` renders the sources in `corefiles/` and `skills/` into one directory
per harness, then syncs that directory outward. The snapshot is what actually
lands on a machine.

```
snapshots/
  claude/   AGENTS.md, CLAUDE.md, skills/<name>/SKILL.md
  codex/    AGENTS.md, skills/<name>/SKILL.md
```

Two reasons this exists instead of syncing `skills/` directly:

1. **Tiers flatten here, not on the target.** A machine sees one flat
   `skills/` directory, so `rsync --delete` is safe: the snapshot is the complete
   intended state, and anything on the box that is not in it should be removed.
2. **You can read what a harness will get.** Diff a snapshot against the last one
   before you apply, and you know exactly what changes on every box.

Commit snapshots. The diff is the review.
