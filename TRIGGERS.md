# Triggers

Maintained by the master only. Workers register in the `## Registration` block of their
own `projects/<name>/TODO.md`; the master copies the ID here.

**Automated jobs read courier IDs from this table at run time.** Never copy an ID into a
scheduled task's prompt: when a worker replaces its courier, the old ID would keep being
fired.

## Couriers

| Project | Purpose | Courier | State | Since | Old (disabled, not deleted) |
|---|---|---|---|---|---|
| example-blog | Example only: a static blog | `trig_EXAMPLE_COURIER_2` | disabled, fired on demand, hands over to coordinator | 2026-01-20 | `trig_EXAMPLE_COURIER_1` |

## Recurring handoffs

Jobs of the master's own (a scheduled task in the master project) that create tasks on a
schedule and fire the couriers listed above.

| Job | Projects | Rhythm | Master task | Data from |
|---|---|---|---|---|
| Weekly link check | example-blog | Mondays 09:00 UTC | `trig_EXAMPLE_WEEKLY` | collector on own server, `reports/links/` (see docs/patterns.md) |
