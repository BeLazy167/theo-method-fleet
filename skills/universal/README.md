# Universal skills

Skills every machine gets. They operate *within* a project, so they are useful wherever work happens.

Installs into `~/.claude/skills/` and `~/.codex/skills/` on every box.

Current members:

- `file-pr` — opens a PR with a title that says why and a description that opens with the problem.
- `babysit-pr` — loops on an open PR until CI is green and reviewers approve.
- `file-upload` — uploads a local file to `files.tslop.org` and returns its public URL.
- `html-communication` — writes a readable HTML artifact and publishes it through PostPlan.
- `postplan-read` — reads a PostPlan URL with the shell instead of a browser.

A skill belongs here when it would still make sense on a box you have not built yet. If it only makes sense on one machine, it goes in `command-center/`. If it depends on one harness, keep it here but narrow `metadata.harness`.
