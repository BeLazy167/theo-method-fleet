---
name: apply-fleet
description: Use when the user asks to apply, sync, push, or roll out fleet changes, skills, or core agent files to their machines.
metadata:
  harness: [claude, codex]
  platform: [darwin, linux]
  scope: command-center
---

# Apply the fleet

Render this repo into per-harness snapshots, then sync each snapshot to the
machines that should have it.

## Rules

1. Read `boxes/machines.json` first. It says which harnesses and which scope each
   machine takes.
2. Commit and push this repo before syncing. Never sync a dirty tree. If it is
   dirty, stop and tell the user.
3. Sync is one-way, repo to machine. Never pull edits back. If a box has local
   changes, report them and stop.
4. Deletions propagate. Name every skill that will disappear before you run it.

## Routing

Every `SKILL.md` carries its own routing in frontmatter:

```yaml
metadata:
  harness: [claude, codex]      # which harnesses get it
  platform: [darwin, linux]     # which OSes it can run on
  scope: fleet                  # fleet = every box, command-center = leader only
```

The directory a skill lives in is for humans. `metadata` is what decides where it
lands. When the two disagree, the metadata is right and the directory should move.

## Render

Build one snapshot per harness. This flattens the tiers, so each machine sees a
single skills directory.

```
snapshots/<harness>/
  AGENTS.md              from corefiles/AGENTS.md
  CLAUDE.md              claude only
  skills/<name>/SKILL.md every skill whose metadata matches
```

Include a skill in `snapshots/<h>/` when `<h>` is in `metadata.harness`. Exclude
`scope: command-center` skills from every snapshot that is going to leave this
machine — they install locally and never travel.

## Sync

```bash
rsync -az --delete snapshots/<harness>/ <host>:~/.<harness>/
```

`--delete` is safe here because the snapshot is the complete intended state of
that directory. Do not run `--delete` against a directory you assembled from more
than one source.

Skip a machine when the platform does not match, and say that you skipped it.

## After

Verify one machine, not all of them. List the skills directory over SSH and
confirm the skill you just changed is there with the new description. Then report
which boxes updated, which were skipped, and why.
