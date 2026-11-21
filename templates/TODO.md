# Relay: <name>

Only the worker "<name>" writes this file (plus the master when adding a TODO).
Format: [docs/todo-format.md](../../docs/todo-format.md)
Open tasks: `grep -E '^## TODO-[0-9]+ · open' projects/<name>/TODO.md`

## Registration
- **Courier:** _(not registered yet)_
- **Registered:** –
- **Model:** –
- **Old couriers (disabled, not deleted):** –

## Feedback
- **Mode:** _(push / poll / none, set by the master)_
- **Reporter:** _(set by the master during setup)_
- **Rule:** In mode `push`, whoever sets a task to `done` or `blocked` fires the reporter once (fire_trigger, no extra text) after pushing. In `poll` and `none`, do nothing extra.

## Tasks

## TODO-001 · open
- **Date:** <YYYY-MM-DD>
- **From:** master (setup)
- **Task:** Confirm the relay setup: the courier handed this task to you (the project coordinator), you can push to `main`, and the Registration block above is filled in.
- **Limits:** Only change this file.
- **Result wanted:** Courier ID, whether it is disabled, whether this handoff reached you, whether push to `main` works.

### Result
_(courier adds the handoff line, then the coordinator adds the result, ending with a “Times (verified):” line, see docs/todo-format.md#times)_
