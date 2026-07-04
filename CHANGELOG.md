# Changelog

All notable changes to this skill are documented here, including anything an
agent or user must do when upgrading. Format follows
[Keep a Changelog](https://keepachangelog.com/); versions follow
[SemVer](https://semver.org/) and match `version` in `SKILL.md` frontmatter.

Read this before upgrading. General upgrade rules:

- **OpenClaw hook changes only take effect after re-copying and restarting.**
  The hook runs from a copy: re-run
  `cp -r hooks/openclaw ~/.openclaw/hooks/self-improvement` and restart the
  gateway after upgrading.
- `.learnings/` files are user data and are never migrated or overwritten by
  upgrades; first-use initialisation is idempotent and only creates missing
  files.

## [Unreleased]

## [0.2.0] - 2026-07-04

### Added

- **OpenClaw session-end error sweep** (`hooks/openclaw/`): the hook now also
  fires on `command:new` / `command:reset`, scans the ended session's
  transcript for the same error patterns as `scripts/error-detector.sh`, and
  appends pending entries (`Source: openclaw-error-sweep`) to
  `<workspace>/.learnings/ERRORS.md`. Opt-in: runs only when
  `<workspace>/.learnings/` exists. Excerpts are capped, truncated, redacted,
  and deduplicated.
- Pending-triage note in the bootstrap reminder when auto-detected error
  entries await review.
- Test suite: `node --test hooks/openclaw/handler.test.js` (no dependencies).
- Docs: "Error Detection on OpenClaw" section and platform support matrix in
  `references/openclaw-integration.md`; uninstall guide in
  `references/uninstall.md`; this changelog.

### Changed

- `hooks/openclaw/HOOK.md` events metadata is now
  `["agent:bootstrap", "command:new", "command:reset"]`.
- Docs now state explicitly that `scripts/error-detector.sh` is Claude Code
  only (OpenClaw has no `PostToolUse` equivalent).

### Upgrade notes (0.1.x → 0.2.0)

1. Re-copy the hook and restart the gateway (see general rules above).
2. To enable the new error sweep: `mkdir -p ~/.openclaw/workspace/.learnings`.
   Without that directory, behavior is identical to 0.1.0.
3. No breaking changes; no `.learnings/` migration needed.

## [0.1.0] - 2026-01-31

### Added

- Initial release: `SKILL.md` with logging formats (`LEARNINGS.md`,
  `ERRORS.md`, `FEATURE_REQUESTS.md`), promotion workflow, and skill
  extraction.
- OpenClaw bootstrap-reminder hook (`agent:bootstrap`).
- Claude Code hook scripts: `activator.sh` (UserPromptSubmit) and
  `error-detector.sh` (PostToolUse).
- Reference guides for hooks setup and OpenClaw integration.
