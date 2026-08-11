# How to set up a box

This is the authoritative fleet-local runbook. Read `boxes/machines.json` before
changing a machine, and update this file when the shared baseline changes.

## Before provisioning

1. Confirm the target host and that non-interactive key-based SSH works:
   `ssh -o BatchMode=yes <host> true`.
2. Decide whether this is a new machine or a repair of an existing inventory
   entry. Preserve working machine-specific choices.
3. Confirm the role, platform, harnesses, shell, sudo availability, and whether
   Tailscale works.

## Target state

- Stable hostname and SSH alias.
- Tailscale/MagicDNS where supported.
- Public-key SSH that works with `BatchMode=yes`.
- A main tmux session whose status color matches the machine inventory.
- The fleet's baseline Git, shell, runtime, GitHub CLI, and harness tooling.
- Claude Code and Codex authenticated on the machine itself.

## Skills and shared configuration

- Install `skills/universal/` into both configured harnesses.
- Install `skills/claude-only/` into Claude only.
- Install nothing from `skills/command-center/` on the target.
- Honor every skill's `metadata.harness`, `metadata.platform`, and credential
  requirements.
- Shared settings and agents may come from rendered snapshots. Install skill
  source from `skills/`, not a potentially stale snapshot mirror.

## Registration

1. Add or update the machine in `boxes/machines.json`.
2. Confirm `ccusage-fleet` includes the host.
3. Record the exact remote `ccusage` invocation. A login-shell runtime manager
   may require `zsh -lic` or the target's equivalent.
4. Update any human-facing inventory/dashboard generated from the machine file.

## Completion

Verify SSH, tmux color, each configured harness, installed skill names, and one
`ccusage-fleet` query. Report what was installed, what was preserved or skipped,
and any authentication the user must finish.
