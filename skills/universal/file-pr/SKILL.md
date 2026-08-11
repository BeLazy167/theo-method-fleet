---
name: file-pr
description: Use when the user asks to file, open, create, or submit a PR or pull request.
metadata:
  harness: [claude, codex]
  platform: [darwin, linux]
  scope: fleet
---

# File a pull request

Write PRs a human wants to read. The title says why. The description opens with the problem, then the fix.

## Before filing

1. Check whether a PR for this branch already exists. If it does, update it instead.
2. Diff the branch against `origin/main` locally. Confirm the contents match the goal in the user's original prompt. Drop anything that crept in.
3. Read the last few merged PRs and recent commit messages. Follow that repo's title convention. Titles usually become squash commit messages.

## Titles

Concise, human readable, explains why the change matters.

BAD
> ❌ auth: refactor token refresh middleware

GOOD
> ✅ auth: stop logging users out mid-session on token refresh

## Descriptions

Open with a plain explanation of the problem, phrased from the user's original prompt. Then explain the fix in one or two sentences. Do not lead with an inventory of what you changed.

BAD
> ❌ Removed the implicit cache-key fallback from `resolveEntity` and every downstream caller (`getUser`, `getOrg`, `listMembers`, the batch loader). Key construction moves into `CacheKey.from()`, which now takes an explicit tenant argument. Deleted `legacyKeyFor`, `withImplicitTenant`, and the v1 resolver's fallback branch. Updated 14 call sites and their fixtures.

GOOD
> ✅ Two users hitting the same endpoint could see each other's results, because the cache key left out the tenant. It is now part of the key.

Keep the body short. Add a `## Testing` line stating what you actually ran. If you did not run it, say so.

Screenshots and videos help. Use the `file-upload` skill to host them and embed the returned URL.

## Rules

- Open a real PR, not a draft, so review bots run. Draft only if the user asks.
- Do not let the PR grow past the user's original goal.
- End the description with a line naming the model and harness that wrote it: `_Filed by claude-opus-5 via Claude Code._`
- If the user also asked you to watch it, continue with the `babysit-pr` skill.
