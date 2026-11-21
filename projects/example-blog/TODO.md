# Relay: example-blog

Only the worker "example-blog" writes this file (plus the master when adding a TODO).
Format: [docs/todo-format.md](../../docs/todo-format.md)
Open tasks: `grep -E '^## TODO-[0-9]+ · open' projects/example-blog/TODO.md`

> This is a made-up example showing all four states. Delete the folder once your own
> projects are set up.

## Registration
- **Trigger:** trig_EXAMPLE0000000000
- **Registered:** 2026-01-10 09:12 (UTC)
- **Model:** small model for the check, stronger subagent for the work
- **Notes:** push to main works

## Tasks

## TODO-004 · open
- **Date:** 2026-01-16
- **From:** master (on behalf of the owner)
- **Task:** Add an RSS feed for the blog posts at `/feed.xml`.
- **Limits:** No new dependency if the site generator can do it natively. No deploy.
- **Result wanted:** PR link, feed URL on the preview build.

### Result
_(filled in by the worker)_

## TODO-003 · in progress
- **Date:** 2026-01-15
- **From:** master (on behalf of the owner)
- **Task:** Shorten all page titles longer than 60 characters, keep the meaning.
- **Limits:** Only titles, no body text. No deploy.
- **Result wanted:** Table old → new title, PR link.

### Result
_(filled in by the worker)_

## TODO-002 · blocked
- **Date:** 2026-01-14
- **From:** master (on behalf of the owner)
- **Task:** Collect visitor numbers for the last 30 days.
- **Limits:** Read only. No new tracking, no new access.
- **Result wanted:** Table date | visitors | page views, sums for 7/30 days, source.

### Result
**2026-01-14 10:05 (UTC):** blocked, no read access from this session.
- The only source is the server's access log; this session has no access to the server.
- No tracking script in the site, so no analytics export either.

**Question for the owner: Which source should I read?**
1. Run this task on your own machine via remote session, read the log only. Recommended.
2. Give a read-only analytics token as an environment secret of this project
   (access change, needs your explicit yes in this chat).

## TODO-001 · done
- **Date:** 2026-01-10
- **From:** master (setup)
- **Task:** Confirm the relay setup: read this file, check that you can push to `main`, and register your trigger above if you have not yet.
- **Limits:** Only change this file.
- **Result wanted:** Trigger ID, model of the scheduled task, whether push to `main` works.

### Result
**2026-01-10 09:14 (UTC):** done.
- Scheduled task "relay-check example-blog" created and disabled, ID in Registration.
- Model: small model as requested.
- Push to `main` works.
