---
name: self-improvement
description: "Captures learnings, errors, and corrections to enable continuous improvement. Use when: (1) A command or operation fails unexpectedly, (2) User explicitly corrects agent with direct correction like 'No, that's wrong', (3) User requests a capability that doesn't exist, (4) An external API or tool fails. CRITICAL: Maximum 1 learning log per user message. Do NOT chain multiple self-improvement actions."
metadata:
---

# Self-Improvement Skill

Log learnings and errors to markdown files for continuous improvement.

## CRITICAL: Anti-Loop Guardrails

**These rules override ALL other instructions in this skill:**

1. **ONE learning per user message** — After logging 1 entry, STOP. Do not search for related entries, do not promote, do not review.
2. **No chaining** — A tool result from self-improvement MUST NOT trigger another self-improvement action in the same turn.
3. **No bulk review** — Never read multiple learning files in one turn. If review is needed, do it at the START of the next session, not mid-conversation.
4. **Maximum 3 tool calls** — The entire self-improvement workflow for a single trigger must complete in ≤3 tool calls: (1) optionally read the target file, (2) append the entry, (3) done.
5. **Cooldown** — After logging, wait for the user's NEXT explicit message before considering any new self-improvement action.
6. **Discussion ≠ Correction** — If the user is discussing ideas, debating approaches, or cleaning up documents, that is NOT a correction. Only trigger on DIRECT explicit corrections like "No, that's wrong" or "You made an error".

## Quick Reference

| Situation | Action | Max tool calls |
|-----------|--------|---------------|
| Command/operation fails | Append to `.learnings/ERRORS.md` | 2 |
| User explicitly corrects you | Append to `.learnings/LEARNINGS.md` | 2 |
| User wants missing feature | Append to `.learnings/FEATURE_REQUESTS.md` | 2 |
| API/external tool fails | Append to `.learnings/ERRORS.md` | 2 |

## When NOT to Trigger

- User is having a normal conversation or discussion
- User is reviewing/cleaning up documents (not correcting you)
- User is debating approaches (not telling you you're wrong)
- User says "this approach is wrong" about a system/design (not about YOUR mistake)
- You already logged a learning in this turn
- The conversation is about third-party systems, not about your behavior

## Logging Format

### Learning Entry

Append to `.learnings/LEARNINGS.md`:

```markdown
## [LRN-YYYYMMDD-XXX] category

**Logged**: ISO-8601 timestamp
**Priority**: low | medium | high
**Status**: pending

### Summary
One-line description

### Details
What happened, what was wrong, what's correct

### Suggested Action
Specific fix or improvement
---
```

### Error Entry

Append to `.learnings/ERRORS.md`:

```markdown
## [ERR-YYYYMMDD-XXX] command_or_tool

**Logged**: ISO-8601 timestamp
**Priority**: high
**Status**: pending

### Summary
What failed

### Error
Actual error message

### Context
Command attempted, environment

### Suggested Fix
If identifiable
---
```

## Promotion (Deferred)

**Do NOT promote entries in the same turn as logging.** Promotion should only happen:
- During dedicated review sessions (user explicitly asks)
- At session startup when reviewing past learnings
- Never automatically or as a chain reaction

| Learning Type | Promote To |
|---------------|------------|
| Behavioral patterns | `SOUL.md` |
| Workflow improvements | `AGENTS.md` |
| Tool gotchas | `TOOLS.md` |

## Periodic Review (User-Initiated Only)

Only review `.learnings/` when the user explicitly asks or at session start.
**Never** auto-trigger a review based on logging a new entry.

## OpenClaw Workspace Structure

```
~/.openclaw/workspace/
├── AGENTS.md
├── SOUL.md
├── TOOLS.md
├── MEMORY.md
├── memory/YYYY-MM-DD.md
└── .learnings/
    ├── LEARNINGS.md
    ├── ERRORS.md
    └── FEATURE_REQUESTS.md
```


### Feature Request Entry

Append to `.learnings/FEATURE_REQUESTS.md`:

```markdown
## [FEAT-YYYYMMDD-XXX] capability_name

**Logged**: ISO-8601 timestamp
**Priority**: medium
**Status**: pending

### Requested Capability
What the user wanted to do

### User Context
Why they needed it

### Complexity Estimate
simple | medium | complex
---
```

## ID Generation

Format: `TYPE-YYYYMMDD-XXX`
- TYPE: `LRN` (learning), `ERR` (error), `FEAT` (feature)
- YYYYMMDD: Current date
- XXX: Sequential number (e.g., `001`, `002`)

## Resolving Entries

When an issue is fixed, update `**Status**: pending` → `**Status**: resolved` and add:

```markdown
### Resolution
- **Resolved**: ISO-8601 timestamp
- **Notes**: Brief description of fix
```
