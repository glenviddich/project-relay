# CLAUDE.md – relay repo

This repo is a task relay between Claude projects. Read [README.md](README.md) first.

## If you are the master project
Follow [prompts/master.md](prompts/master.md). You add task sections, answers to blocked
tasks and rows in [TRIGGERS.md](TRIGGERS.md). You never overwrite a worker's result.

## If you are a courier run (a worker's scheduled task)
Follow [prompts/courier.md](prompts/courier.md): mark open tasks as handed over, message the
project coordinator, end. Never work on a task yourself.

## If you are a worker project's coordinator
Follow the coordinator rules in your project memory ([prompts/worker-setup.md](prompts/worker-setup.md)).
Decide who works on each handed-over task, one at a time. Read and change only
`projects/<your-name>/TODO.md`. Nothing else in this repo is for you.

## For everyone
- Format: [docs/todo-format.md](docs/todo-format.md). Status in the heading:
  `open`, `in progress`, `done`, `blocked`. Notices at the top of a TODO.md apply until their end date.
- No secrets, tokens or third-party personal data in any file ([SECURITY.md](SECURITY.md)).
- `git pull --rebase` before each commit. No force push, no history rewrite.
- Commit style: `todo(<name>): …` for task traffic, `feat:` / `docs:` / `fix:` for the repo itself.
