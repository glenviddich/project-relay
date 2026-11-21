# TODO format

One file per worker: `projects/<name>/TODO.md`. The folder name is the project's ID
everywhere (commits, branches, trigger table). Use lowercase letters, digits and `-`.

## File layout

```markdown
# Relay: <name>

Only the worker "<name>" writes this file (plus the master when adding a TODO).
Format: docs/todo-format.md

> ⚠️ **Notice (valid until 2026-01-31):** CI quota used up. Run tests locally, do not
> wait for CI checks. Normal procedure again from 2026-02-01.

## Registration
- **Courier:** trig_XXXXXXXXXXXXXXXX
- **Registered:** 2026-01-10 09:12 (UTC)
- **Model:** smallest available
- **Old couriers (disabled, not deleted):** –

## Feedback
- **Mode:** push
- **Reporter:** trig_XXXXXXXXXXXXXXXX
- **Rule:** In mode `push`, whoever sets a task to `done` or `blocked` fires the reporter once after pushing.

## Tasks

## TODO-002 · open
...

## TODO-001 · done
...
```

- **Notices** (optional) sit right under the title. They hold temporary rules that apply
  to every task until their end date. Only the master adds and removes them. See
  [patterns.md](patterns.md#temporary-rules-with-an-end-date).
- **Registration** is written by the worker during setup. The master reads the courier ID
  from here and copies it into `TRIGGERS.md`. When the worker replaces its courier, it
  moves the old ID to “Old couriers” and keeps that task disabled.
- **Feedback** is written by the master (setup or a mode change), never by the worker. It
  tells whoever finishes a task whether to fire the master's reporter (mode `push`) or not
  (`poll`, `none`). See [how-it-works.md](how-it-works.md#feedback-channel).
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
| `open` | new or answered, waits for the courier | master / owner |
| `in progress` | courier handed it to the project coordinator; someone works on it | courier |
| `done` | finished, result written | whoever the coordinator picked |
| `blocked` | needs an answer (question in the result), or the handoff failed | worker / courier |

Find open tasks:

```bash
grep -E '^## TODO-[0-9]+ · open' projects/<name>/TODO.md
```

## The handoff line

The courier writes one line as the first entry under `### Result`, and sets the status to
`in progress`:

```markdown
- **Handed to project coordinator:** 2026-01-15 13:40 (UTC), courier run. The coordinator decides who works on it.
```

It stays there when the result is added. For the master it means: the task has arrived,
do not fire the courier again. If the coordinator could not be reached, the courier
writes instead, and sets `blocked`:

```markdown
- **Handoff to project coordinator failed:** 2026-01-15 13:40 (UTC), tool not available. Please start it in the project chat.
```

## Writing a result

Below the handoff line, whoever did the work adds a block starting with a timestamp:

```markdown
**2026-01-15 14:03 (UTC):** done.
- What was done, in 2–5 bullets.
- Links: PR, commit, artifact, file.
- What is still open.
- **Times (verified):** handed over 13:40:05 (commit 1a2b3c4); fix pushed 13:58:12 (commit 5d6e7f8); PR opened 14:01 (PR #12). All UTC.
```

No secrets, no tokens, no personal data of third parties.

## Times

A report is only as good as its times, and Claude sessions are tempted to guess them.

- **Every time** in a result, a handoff line or a report comes from a **verifiable source**:
  a commit timestamp (`git log -1 --format=%cI <sha>`), a log line, a chat or PR timestamp.
  For your own action right now, take it from `date -u`.
- **Never** from a file's modification time in a clone. A fresh clone or `git pull` sets
  every mtime to the moment of the pull, not to when the content changed.
- **Never** estimated or reconstructed (“about an hour ago”). If no source exists, write
  `time unknown`.
- **Every result** carries one line `**Times (verified):**` listing each step with its
  time and source, e.g. `dropped 03:21:31 (commit abc1234)`. The master copies times from
  there and never has to look them up. Steps without a source: `time unknown`.
- Write the time zone once per line (UTC is the default in this template).

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
| courier hands over | `todo(<name>): TODO-NNN handed to coordinator` (several IDs comma-separated) |
| work finished | `todo(<name>): TODO-NNN done` / `… blocked` |
| worker registers its courier | `todo(<name>): register courier` |
| master adds or removes a notice | `todo(<name>): notice …` |
