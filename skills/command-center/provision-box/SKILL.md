---
name: provision-box
description: Use when the user adds a new machine, reimages one, or asks to set up, provision, or bootstrap a box or dev server over SSH.
metadata:
  harness: [claude, codex]
  platform: [darwin, linux]
  scope: command-center
---

# Provision a box

Bring a fresh machine to the shared baseline over SSH.

This runs from the machine doing the provisioning, not the one being provisioned.
Every command below targets `<host>`. If you find yourself running one locally,
you are on the wrong box — stop.

## Before you start

1. Get the hostname or Tailscale name and confirm SSH works: `ssh <host> true`.
2. Read `boxes/machines.json`. If the host is already listed, this is a
   re-provision. Diff against the baseline instead of reinstalling everything.
3. Confirm the role with the user: dev box, build box, or headless runner. The
   role decides which skill tiers get synced.

## Baseline

Install, in this order, and skip anything already present:

- Package manager refresh, then `git`, `curl`, `ripgrep`, `fd`, `jq`, `tmux`, `zsh`.
- `mise` for runtimes. Pin `node` and `python` per repo, not globally.
- `tailscale`, then `tailscale up`. The user completes the auth link, not you.
- The user's dotfiles repo, cloned and linked. Never overwrite an existing dotfile;
  back it up first.
- `gh`, then confirm auth. If it is not authenticated, tell the user to run
  `gh auth login` themselves. Do not paste tokens.
- tmux config with mouse mode on, and the machine's assigned color in the status
  bar so SSH sessions are distinguishable at a glance.

## Then

1. Sync skills with the `apply-fleet` skill. A new box gets the `universal` tier
   plus any tier its role requires. It does not get `command-center`.
2. Add the machine to `boxes/machines.json`: name, color, specs, role, connection
   method, harnesses.
3. Report what you installed, what you skipped, and anything the user must finish
   by hand.

## Rules

- Secrets never travel in a command you write. Ask the user to place them.
- Do not reboot without asking.
- If a step fails, stop and report. Do not retry with `sudo` and hope.
