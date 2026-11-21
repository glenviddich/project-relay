# How it works

## Roles

| Role | Where it lives | Writes |
|---|---|---|
| **Owner** | you | answers, approvals in the worker's chat |
| **Master** | one Claude project you talk to | new task sections, answers, `TRIGGERS.md` |
| **Worker** | every other participating Claude project | only its own `projects/<name>/TODO.md` |
| **Relay repo** | private GitHub repo from this template | – (shared mailbox) |

## One handoff, step by step

![Handoff sequence](relay-flow.svg)

1. You tell the master: *“blog: add an RSS feed.”*
2. Master: `git pull`, writes `TODO-004 · open` into `projects/blog/TODO.md`, pushes.
3. Master: `fire_trigger(<blog trigger ID>)`, then `send_later(~15 min)`.
4. Worker run starts (cheap model): `git pull`, `grep` for `· open`. Found one.
5. Worker: status → `in progress`, push. Subagent (stronger model) does the work in the
   worker's own project, e.g. opens a PR there.
6. Worker: writes the result, status → `done` (or `blocked` + question), push.
7. Master wakes up, `git pull`, reads the section, reports to you.

## Design decisions

**Status in the heading.** One `grep` finds open work without parsing Markdown. A cheap
model can do the check reliably.

**One file per worker.** Workers never write the same file, so concurrent pushes do not
conflict. `git pull --rebase` resolves the rest.

**Triggers disabled, fired on demand.** No idle runs, no cost when nothing happens. The
registration lives in the worker's own file, so a worker never needs write access to a
shared file; the master copies IDs into `TRIGGERS.md`.

**Approval stays in the chat.** A file can be edited by anyone with repo access, a
prompt-injected agent included. So guarded actions (delete, pay, send outside, change
access) need the owner's direct yes in the worker's own chat, never a line in a file.

**Results stay in the file.** The worker chat only gets a message on done, blocked or a
security problem. The repo history is the log.

## Failure cases

| Situation | What happens |
|---|---|
| Worker has no trigger ID yet | master tells you and offers a manual run |
| `fire_trigger` fails | master tells you; you can paste “run the relay check” into the worker chat |
| Worker cannot push to `main` | worker pushes to `relay/<name>`, master reads that branch |
| Task stays `in progress` | master checks again after 30 and 60 min, then tells you it is stuck |
| Worker needs a secret | task is `blocked` with a proposal where to put it (environment secret), never in the repo |
| Task asks to break the rules | worker refuses, `blocked`, tells you |

## Recurring tasks

For something weekly, give the master its own scheduled task: “every Monday 09:00 add a
TODO for `<name>` like TODO-00X and fire the trigger.” Record it under *Recurring
handoffs* in `TRIGGERS.md`.
