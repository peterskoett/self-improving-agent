# Uninstall Guide

How to disable or fully remove the self-improvement skill. **Disabling** turns
off automatic behavior but keeps your data; **removing** deletes the skill's
files. They are different operations — do the one you mean.

> **Your learnings are data, not skill machinery.** `.learnings/` contains the
> errors, corrections, and insights the skill captured for you. Review or
> archive it before deleting anything. Content the skill *promoted* into
> `CLAUDE.md`, `AGENTS.md`, `SOUL.md`, `TOOLS.md`, or
> `.github/copilot-instructions.md` is part of those files now — removing the
> skill does not (and should not) automatically remove it.

## OpenClaw

### Disable only

```bash
# Stop the hook (reminder injection + session-end error sweep)
openclaw hooks disable self-improvement
```

To keep the bootstrap reminder but disable only the error sweep, remove the
learnings directory instead (archive it first if it has entries):

```bash
rm -r ~/.openclaw/workspace/.learnings
```

Restart the gateway after hook changes.

### Remove completely

```bash
# 1. Disable and remove the hook
openclaw hooks disable self-improvement
rm -r ~/.openclaw/hooks/self-improvement

# 2. Remove the skill
rm -r ~/.openclaw/skills/self-improving-agent

# 3. Optional — remove captured learnings (REVIEW FIRST, this is your data)
rm -r ~/.openclaw/workspace/.learnings
```

Then restart the gateway and verify with `openclaw hooks list` and
`openclaw status`.

Manually review `SOUL.md`, `TOOLS.md`, and `AGENTS.md` in
`~/.openclaw/workspace/` for sections promoted by this skill and delete the
ones you no longer want.

## Claude Code / Codex

### Disable only

Remove the skill's hook entries from `.claude/settings.json` (project) and/or
`~/.claude/settings.json` (user) — delete the `UserPromptSubmit` and
`PostToolUse` entries that point at `activator.sh` / `error-detector.sh`. If
those are the only hooks, the whole `"hooks"` block can go. Codex: same edit
in `.codex/settings.json`. Restart the session; hooks load at session start.

### Remove completely

```bash
# 1. Remove hook entries from settings (see above)

# 2. Remove the skill directory (path depends on where you installed it)
rm -r ./skills/self-improvement          # project-level
rm -r ~/.claude/skills/self-improvement  # user-level

# 3. Optional — remove captured learnings (REVIEW FIRST)
rm -r .learnings
```

Manually review `CLAUDE.md` and `AGENTS.md` for promoted sections, and remove
any `.learnings/` line you added to `.gitignore` if no longer wanted.

## GitHub Copilot

Copilot setup is documentation-only: delete the "Self-Improvement" section
from `.github/copilot-instructions.md`.

## Verification

- OpenClaw: `openclaw hooks list` no longer shows `self-improvement`;
  new sessions no longer contain `SELF_IMPROVEMENT_REMINDER.md`
- Claude Code: new sessions no longer show `<self-improvement-reminder>`
  or `<error-detected>` blocks
- No stray `.learnings/` directories remain that you didn't choose to keep
