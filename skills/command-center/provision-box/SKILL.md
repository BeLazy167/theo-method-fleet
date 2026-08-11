---
name: provision-box
description: Use when the user adds a machine, asks to provision a box, says "make this feel like my other machines", or asks what setup is still missing.
metadata:
  harness: [claude, codex]
  platform: [darwin, linux]
  scope: command-center
  requires: "A local clone of the fleet repo; SSH access to the target machine"
---

# Provision a box

This skill locates the fleet's authoritative setup instructions; it does not
replace them.

1. Read `HOW_TO_SET_UP_A_BOX.md` from the local fleet clone.
2. Read the target entry in `boxes/machines.json`, if one exists.
3. Follow the runbook over non-interactive, key-based SSH.
4. Preserve per-machine exceptions and authenticate each harness on the target
   itself.
5. Stop and report if the runbook, inventory, and live machine disagree. Update
   the authoritative source before continuing rather than improvising a fourth
   configuration.
