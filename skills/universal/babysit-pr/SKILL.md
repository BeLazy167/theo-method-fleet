---
name: babysit-pr
description: Use when the user asks to babysit, monitor, watch, shepherd, or land a PR, or to get a PR green.
metadata:
  harness: [claude, codex]
  platform: [darwin, linux]
  scope: fleet
---

# Babysit a pull request

Loop until CI is green and every required reviewer has approved. Report back
when it lands or when you need a decision.

## The loop

1. If the harness has PR monitoring tools, use them so you react when comments arrive.Otherwise poll every few minutes with `gh pr view` and `gh pr checks`.

2. Only act on checks and comments newer than the latest push. Ignore stale ones.
3. Verify every bot finding against the source before you change code. Review bots are helpful and often wrong.
4. Fix real findings and real CI failures. Tell repository failures apart from infrastructure flakes. Re-run flakes once; do not "fix" them with code.
5. Watch `main`. Rebase when the branch goes stale or conflicts appear.
6. Repeat until green and approved.

## Replying

When you dismiss a finding, reply with the reason and resolve the comment. Do
not silently ignore it.

Format every comment left on the user's behalf like this:

```
**<model-slug> responding on behalf of <your name>**

<reply>
```

## Rules

- Do not let review feedback expand the PR beyond the user's original goal. Address real shortcomings. Refuse scope creep and say why in the thread.
- Not every comment is mission critical. Weigh it against the size of the PR.
- If another PR lands and makes this one obsolete, stop monitoring, report to the user, and ask before closing. Close only if the user authorized it.
- Never merge unless the user asked you to merge.
