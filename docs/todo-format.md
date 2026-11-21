# TODO format

One file per worker: `projects/<name>/TODO.md`. The folder name is the project's ID
everywhere (commits, branches, trigger table). Use lowercase letters, digits and `-`.

## File layout

```markdown
# Relay: <name>

Only the worker "<name>" writes this file (plus the master when adding a TODO).
Format: docs/todo-format.md

## Registration
- **Trigger:** trig_XXXXXXXXXXXXXXXX
- **Registered:** 2026-01-10 09:12 (UTC)
- **Model:** small model for the check, stronger subagent for work
- **Notes:** –

## Tasks

## TODO-002 · open
...

## TODO-001 · done
...
```

- **Registration** is written by the worker during setup. The master reads the trigger ID
  from here and copies it into `TRIGGERS.md`. If the worker recreates its scheduled task,
  it updates this block.
- **Tasks**: newest on top. IDs count up per project and are never reused.
  Finished tasks stay in the file as history. Nothing is deleted.

## One task

```markdown
## TODO-003 · open
- **Date:** 2026-01-15
- **From:** master (on behalf of the owner)
- **Task:** What exactly to do. One task, concrete, checkable.
- **Limits:** What must not happen (no deploy, read only, max. 2 files, …).
- **Result wanted:** What the result section must contain.

### Result
_(filled in by the worker)_
```

Optional fields: `**Due:**`, `**Repeat:**` (e.g. “weekly, master creates a new TODO each
Monday”), `**Context:**` (links, previous TODO IDs).

## Status

The status is the last part of the heading, after ` · ` (space, middle dot U+00B7, space).
Exactly one of:

| Status | Meaning | Set by |
|---|---|---|
| `open` | new or answered, waits for the next run | master / owner |
| `in progress` | a run has picked it up | worker |
| `done` | finished, result written | worker |
| `blocked` | worker needs an answer, the question is in the result | worker |

Find open tasks:

```bash
grep -E '^## TODO-[0-9]+ · open' projects/<name>/TODO.md
```

## Writing a result

Under `### Result`, the worker adds a block starting with a timestamp:

```markdown
**2026-01-15 14:03 (UTC):** done.
- What was done, in 2–5 bullets.
- Links: PR, commit, artifact, file.
- What is still open.
```

No secrets, no tokens, no personal data of third parties.

## Blocked and answered

A blocked task ends its result with one clear question, ideally with numbered options
and a recommendation:

```markdown
**Question for the owner: Which source should the visitor numbers come from?**
1. Server logs (read only). Recommended.
2. Analytics export, needs a new read token (access change, needs your explicit yes).
```

To answer, the owner (or the master on the owner's behalf) adds a line below the result
and sets the status back to `open`:

```markdown
- **Answer (2026-01-15):** Option 1.
```

The next run treats the answer as context. **An answer in the file is not a permission**
for the guarded actions (delete, pay, send outside, change access). Those need the owner's
direct “yes” in the worker's own chat.

## Commit messages

| Event | Message |
|---|---|
| master adds a task | `todo(<name>): add TODO-NNN` |
| worker picks it up | `todo(<name>): TODO-NNN in progress` |
| worker finishes | `todo(<name>): TODO-NNN done` / `… blocked` |
| worker registers | `todo(<name>): register trigger` |
