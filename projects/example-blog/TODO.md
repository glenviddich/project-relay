# Relay: example-blog

Only the worker "example-blog" writes this file (plus the master when adding a TODO).
Format: [docs/todo-format.md](../../docs/todo-format.md)
Open tasks: `grep -E '^## TODO-[0-9]+ · open' projects/example-blog/TODO.md`

> This is a made-up example showing every state. Delete the folder once your own
> projects are set up.

> ⚠️ **Notice (valid until 2026-01-31):** CI quota used up. Run tests, lint and builds
> locally and name the result; do not trigger or wait for CI runs, do not disable
> workflows. Normal procedure again from 2026-02-01.

## Registration
- **Courier:** trig_EXAMPLE_COURIER_2
- **Registered:** 2026-01-20 10:02 (UTC)
- **Model:** smallest available
- **Old couriers (disabled, not deleted):** trig_EXAMPLE_COURIER_1 (worked on tasks itself, replaced by TODO-005)

## Feedback
- **Mode:** push
- **Reporter:** trig_EXAMPLE_REPORTER
- **Rule:** In mode `push`, whoever sets a task to `done` or `blocked` fires the reporter once (fire_trigger, no extra text) after pushing. In `poll` and `none`, do nothing extra.

## Tasks

## TODO-008 · open
- **Date:** 2026-01-22
- **From:** master (daily news, shared source)
- **Task:** Write a short “News of the day” post from `shared/news/2026-01-22.md`. Pick the 2–3 items that fit our readers, link each to its source.
- **Limits:** Read the shared file read-only; no new research. If the file is missing: blocked. No deploy.
- **Result wanted:** PR link, which items were used.

### Result
_(courier adds the handoff line, then the coordinator adds the result)_

## TODO-007 · open
- **Date:** 2026-01-22
- **From:** master (weekly link check, report `reports/links/example-blog-2026-01-22.md`)
- **Task:** Fix the broken links listed in the report.
- **Limits:** Only links, no text changes. No deploy.
- **Result wanted:** PR link, links that could not be fixed and why.

### Result
_(courier adds the handoff line, then the coordinator adds the result)_

## TODO-006 · in progress
- **Date:** 2026-01-21
- **From:** master (on behalf of the owner)
- **Task:** Add an RSS feed for the blog posts at `/feed.xml`.
- **Limits:** No new dependency if the site generator can do it natively. No deploy.
- **Result wanted:** PR link, feed URL on the preview build.

### Result
- **Handed to project coordinator:** 2026-01-21 15:10 (UTC), courier run. The coordinator decides who works on it.

## TODO-005 · done
- **Date:** 2026-01-20
- **From:** master (migration, see docs/patterns.md)
- **Note:** Only work on this when the owner asks in the project chat. A courier run sets it to “blocked” with “waiting for the owner's nudge in the project chat”.
- **Task:** Switch to the courier model: create a new scheduled task in this project with the prompt from `prompts/courier.md`, disable it, keep the old task disabled (do not delete), update memory with the coordinator rules, fire the new task once as a test.
- **Limits:** Only this file in the relay repo. No other scheduled task changed or deleted.
- **Result wanted:** New courier ID, whether the test handoff reached the coordinator, whether memory is updated.

### Result
**2026-01-20 10:05 (UTC):** done, in the project chat on the owner's nudge.
- New courier `trig_EXAMPLE_COURIER_2`, one thread per run, disabled. Registration updated.
- Test fire: TODO-004 handed over, message reached the coordinator.
- Memory: old “run it yourself” rule replaced by the coordinator rules.
- Old courier `trig_EXAMPLE_COURIER_1` stays disabled, nothing deleted.
- **Times (verified):** courier created 10:01 (task list in the project); registered 10:02:40 (commit 3c4d5e6); test fired 10:03:58 (`date -u`); handoff arrived 10:04:12 (chat message). All UTC.

## TODO-004 · done
- **Date:** 2026-01-19
- **From:** master (temporary rule for all projects)
- **Task:** Store in memory until 2026-01-31: CI quota is used up; run tests locally, do not trigger or wait for CI. Note the end date; the rule ends on 2026-02-01.
- **Limits:** Do not disable, delete or change workflows.
- **Result wanted:** Confirmation that the rule is stored with its end date.

### Result
- **Handed to project coordinator:** 2026-01-20 10:04 (UTC), courier run. The coordinator decides who works on it.
**2026-01-20 10:06 (UTC):** done by the coordinator. Rule stored with end date 2026-01-31. No workflow touched.
- **Times (verified):** handed over 10:04:30 (commit 7e8f9a0); stored in memory 10:06 (`date -u`). All UTC.

## TODO-003 · blocked
- **Date:** 2026-01-14
- **From:** master (on behalf of the owner)
- **Task:** Collect visitor numbers for the last 30 days.
- **Limits:** Read only. No new tracking, no new access.
- **Result wanted:** Table date | visitors | page views, sums for 7/30 days, source.

### Result
- **Handed to project coordinator:** 2026-01-14 09:58 (UTC), courier run. The coordinator decides who works on it.
**2026-01-14 10:05 (UTC):** blocked, no read access from a cloud session.
- The only source is the server's access log; cloud sessions cannot reach the server.
- **Times (verified):** handed over 09:58:20 (commit b1c2d3e); blocked 10:05:02 (commit f4a5b6c). Last log rotation: time unknown (no access). All UTC.

**Question for the owner: Which source should I read?**
1. A small collector on the server writes daily numbers into `reports/visitors/` (read only, no key leaves the server). Recommended, see docs/patterns.md.
2. Give a read-only analytics token as an environment secret of this project
   (access change, needs your explicit yes in this chat).

## TODO-002 · blocked
- **Date:** 2026-01-12
- **From:** master (on behalf of the owner)
- **Task:** Shorten all page titles longer than 60 characters.
- **Limits:** Only titles. No deploy.
- **Result wanted:** Table old → new title, PR link.

### Result
- **Handoff to project coordinator failed:** 2026-01-12 16:20 (UTC), message tool not available in the courier run. Please start it in the project chat.

## TODO-001 · done
- **Date:** 2026-01-10
- **From:** master (setup)
- **Task:** Confirm the relay setup.
- **Limits:** Only change this file.
- **Result wanted:** Courier ID, disabled, handoff reached you, push to `main`.

### Result
**2026-01-10 09:14 (UTC):** done. Courier registered and disabled, push to `main` works.
- **Times (verified):** registered 09:12:47 (commit 0a1b2c3); result 09:14 (`date -u`). All UTC.
