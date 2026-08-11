---
name: audit-agent-history
description: Use when the user wants to know how their agents keep failing, asks to audit past sessions or transcripts, complains that agents keep making the same mistake, or wants evidence before editing their global agent instructions.
metadata:
  harness: [claude]
  platform: [darwin, linux]
  scope: fleet
---

# Audit agent history for failure modes

Do not rewrite global instructions from memory. Read what actually happened, then change the instruction that would have prevented it.

## Where the history lives

Claude Code stores one JSONL transcript per session:

```
~/.claude/projects/<slugified-cwd>/<session-id>.jsonl
```

Each line is a message. Tool calls appear as `tool_use` blocks with `name` and `input`. Results appear as `tool_result`. Codex transcripts, if present, live under `~/.codex/sessions/`.

## The audit

Sample broadly, not just the sessions the user remembers. Count things. A failure that happens once is a story; a failure that happens forty times is an instruction you are missing.

Report per model and per harness, with rates, not raw totals. Sessions are not evenly distributed across models.

Look for at least these:

| Failure mode | How to find it |
|---|---|
| Killed the wrong process | `kill`, `pkill`, `killall` in Bash inputs |
| Broke the environment | `rm -rf`, global installs, edits outside the repo |
| Bypassed checks | `--no-verify`, `--force`, skipped tests |
| PR hygiene | draft PRs, missing descriptions, titles under 5 words |
| Wasted work | repo-wide typechecks and builds for a one-file change |
| Bash error rate | `tool_result` entries marked as errors, over total Bash calls |
| Unasked edits | files changed that the user's prompt never mentioned |
| Stopped early | user's next message is "keep going" or "you didn't finish" |
| Overbuild | new abstractions, config, or files the prompt did not ask for |

Also compute **corrections per 100 user messages**: how often the user says the result is wrong, unwanted, or misread. Split it by kind — wrong result, wrong process, misread request — because the fixes differ.

## Interpret before you act

Rates are biased by how the user works. The hardest tasks go to the strongest model, so that model gets corrected more. A model that never touches UI cannot be corrected on UI. Say this out loud in the report instead of ranking models.

## Then fix the instruction

For each failure mode that recurs, ask:

1. What would have prevented it? A global rule, a repo `AGENTS.md` line, or a skill?
2. Is there already a rule that says it, that the model ignored or never read?
3. Can you show it a bad example and a good example instead of a rule?

Bad and good examples steer models better than prose. Pull the examples from the transcripts you just read.

## Recording the audit

Write the report to a file and keep it. The value is the trend: a failure that appears in three consecutive audits is a rule you are missing, while one that appears once and never again was a bad session, not a bad instruction.

Always record which machines and which date range the audit covered. Rates are meaningless without the denominator, and the next audit needs to compare like with like.

## Interrogate a bad session

When one session clearly went wrong, open it and ask the model directly:

- Why did you decide this was the right direction?
- What in context told you that?
- Categorize every tool call you made into groups. Which groups were wasted?
- You took far longer than this looked like it needed. Where did the time go?

The answer usually names a stale line in `AGENTS.md` or something misread early that stayed in context.
