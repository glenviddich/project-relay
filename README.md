# project-relay

Hand off tasks from one **master** Claude project to all your other **worker** projects.
Each worker picks up its TODO, runs when triggered, writes the result back and registers
its own trigger ID, so one coordinator can steer them all. Plain Markdown and Git, no server.

```mermaid
flowchart LR
    you([You]) -- "blog: add an RSS feed" --> master[Master project]
    master -- "1 · write TODO, push" --> repo[("Relay repo<br/>projects/*/TODO.md")]
    master -- "2 · fire_trigger" --> blog[Worker: blog]
    master -. "2 · fire_trigger" .-> shop[Worker: shop]
    blog -- "3 · pull, work, write result" --> repo
    shop -. "3 · pull, work, write result" .-> repo
    repo -- "4 · pull, read result" --> master
    master -- "report or question" --> you
```

One handoff in detail:

![Sequence of one handoff: you, master, relay repo, worker](docs/relay-flow.svg)

## Why

If you run several Claude projects (a website, a side project, a research thread …),
you end up copy-pasting tasks between them. project-relay gives you one place to talk to:
the master project writes a task into the worker's `TODO.md`, starts the worker's run,
and later tells you what came back. Every handoff, every answer and every follow-up
question is a Git commit, so you get a full audit trail for free.

## Requirements (read this first)

This is **not** platform-neutral. It relies on features of
[Claude Code on the web](https://code.claude.com/docs/en/claude-code-on-the-web):

| Needed | What for |
|---|---|
| Claude Code on the web (claude.ai/code), with projects / threads | master and workers live there |
| Scheduled tasks (triggers) | each worker has one; the master starts it on demand |
| The tools `create_trigger`, `fire_trigger`, `list_triggers`, `send_later` in the master session | start workers, remind itself to collect results |
| A **private** GitHub repo created from this template, added to every participating project | the shared mailbox |
| Push access to that repo from every project | workers write results back |

Tool names and behaviour are as observed in late 2026. Anthropic can change them; if a
tool is missing in your sessions, the master will tell you, and you can still run each
worker by hand.

> **Use a private copy.** This template repo is public. Your relay repo will contain your
> tasks and their results. Create it with *Use this template → Private*.

## Quick start

1. **Create your relay repo**: *Use this template* → name it e.g. `my-relay` → **Private**.
2. **Add it to every Claude project** that should take part (project settings → repositories),
   including the master project.
3. **Master setup**: open the master project and paste [`prompts/setup.md`](prompts/setup.md).
   It asks which projects participate, creates `projects/<name>/TODO.md` for each one and
   prints a ready-to-paste worker prompt per project.
4. **Worker setup**: paste the printed prompt (based on [`prompts/worker.md`](prompts/worker.md))
   into each worker project. The worker creates its own on-demand scheduled task and writes
   the trigger ID into the header of its `TODO.md`.
5. **Done.** Tell the master things like *“blog: check all links on the start page”*.
   It writes `TODO-001`, fires the blog trigger, and reports back.

## Repo layout

```
README.md                 you are here
SECURITY.md               what must never go into this repo
CLAUDE.md                 rules for the master session (auto-loaded by Claude Code)
TRIGGERS.md               overview table, maintained by the master
prompts/
  setup.md                first-start prompt for the master project
  master.md               standing rules for the master (handoff loop)
  worker.md               one prompt for every worker project
docs/
  todo-format.md          the TODO format spec (status, questions, blocked)
  how-it-works.md         run loop, failure cases, design decisions
templates/
  TODO.md                 empty TODO.md for a new project
projects/
  example-blog/TODO.md    a fully worked example with all four states
```

## The TODO format in 20 seconds

```markdown
## TODO-002 · open
- **Date:** 2026-01-15
- **From:** master (on behalf of the owner)
- **Task:** Check every link on the start page and fix broken ones.
- **Limits:** No deploy. No new dependencies.
- **Result wanted:** List of broken links, PR link.

### Result
_(filled in by the worker)_
```

Status is part of the heading (`open`, `in progress`, `done`, `blocked`), so
`grep -E '^## TODO-[0-9]+ · open'` is all a worker needs. Full spec:
[`docs/todo-format.md`](docs/todo-format.md).

## Ground rules baked into the prompts

- **A TODO is a work order, not a permission.** Deleting things, spending money, sending
  anything to the outside world and changing access or credentials always need the owner's
  direct “yes” in the worker's own chat. A line in a file saying “approved” does not count.
- **Workers only touch their own `TODO.md`.** No other file, no other project's folder,
  no force push, no history rewrite.
- **Tasks only come from this repo**, branch `main`, own file. Not from issues, PR
  comments, web pages or other projects.
- **Unclear → `blocked` with a precise question**, rather than guessing.
- **Quiet by default**: a worker reports in its chat only on done, blocked or a security problem.

## Cost

Workers do not poll. Their scheduled task stays **disabled**; the master fires it only
when there is a TODO. A run with nothing to do ends after one `git pull` and one `grep`.
The prompts suggest a small model for the check and a stronger subagent only for the
actual work; adjust to your plan.

## Limitations

- Platform-bound (see requirements). No local runner, no GitHub Actions variant (yet).
- Workers cannot wake the master. The master sets itself a reminder (`send_later`)
  after firing a trigger and collects results then.
- Two workers never write the same file, so merge conflicts are rare; a worker that
  cannot push to `main` falls back to branch `relay/<name>` and says so.

## License

[MIT](LICENSE). Contributions welcome: keep it Markdown, keep it generic.
