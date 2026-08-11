---
name: ccusage-fleet
description: Use when the user asks for Claude or Codex usage, token counts, or estimated costs across the fleet.
metadata:
  harness: [claude, codex]
  platform: [darwin, linux]
  scope: command-center
  requires: "Node.js, npx, and key-based SSH access to the fleet"
---

# ccusage fleet

Aggregate agent usage across the local command center and SSH-connected boxes.
Run this only from the command center. Raw transcripts stay on each box;
`ccusage-fleet` transfers aggregated JSON over SSH.

## Hosts

Read `boxes/machines.json`. Include the local command center as `localhost` and
each reachable SSH host by its configured `host` value. Do not invent or probe
hosts that are not in the inventory.

## Report

Use the period and grouping the user requested:

```bash
npx ccusage-fleet daily --hosts localhost,<host>,<host> --group-by agent
npx ccusage-fleet weekly --hosts localhost,<host>,<host> --group-by device
npx ccusage-fleet monthly --hosts localhost,<host>,<host> --group-by none
```

Add `--since`, `--until`, `--timezone`, `--graph`, or `--graph-metric output`
when requested. Use `--json` when another tool will consume the result.

## Failure handling

- SSH must use configured keys and non-interactive access. Never request or
  transmit machine passwords.
- Report unreachable machines separately. Keep the reachable-machine totals.
- Explain that costs are list-price estimates and subscription billing differs.
- Explain that cache reads dominate total tokens; use output tokens to compare
  work performed.
