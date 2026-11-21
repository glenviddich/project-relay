# First-start prompt (master)

Paste this into the master project once, after adding your private relay repo to it.
Replace `<OWNER>/<RELAY_REPO>`.

```text
Set up a task relay with the GitHub repo <OWNER>/<RELAY_REPO>, created from the
project-relay template. You are the master project.

1. Read README.md, docs/todo-format.md, prompts/master.md and prompts/worker.md in that repo.
2. Ask me which of my projects should take part. For each one I give you a short name
   (folder name: lowercase, digits, "-") and one sentence on what the project is for.
   Ask in one message, wait for my answer.
3. For each project:
   - create projects/<name>/TODO.md from templates/TODO.md (replace <name> and the date),
   - add a row to TRIGGERS.md (trigger "not registered yet", plus the one-line purpose).
   Remove projects/example-blog/ only if I say so; otherwise leave it as reference.
   Commit "feat: add relay projects", push to main.
4. Save the rules from prompts/master.md in your memory, with the relay repo filled in.
5. For each project, print the worker prompt from prompts/worker.md with <NAME> and
   <OWNER>/<RELAY_REPO> filled in, each in its own code block, with a one-line
   instruction: "Add the relay repo to project X, then paste this into its chat."
6. Tell me: once a worker confirms its setup, say "collect triggers" and you will copy
   the trigger IDs from each Registration block into TRIGGERS.md and fire each worker
   once to test TODO-001.

Do not put any secret into the repo. If I give you one, refuse and tell me where it
belongs instead (see SECURITY.md).
```

## What happens next

| Step | Where | Result |
|---|---|---|
| Worker prompt pasted | each worker chat | worker creates a disabled scheduled task, writes its trigger ID into `## Registration`, runs once |
| “collect triggers” | master chat | `TRIGGERS.md` filled, each worker fired once, `TODO-001` comes back `done` |
| Normal use | master chat | “blog: …” → TODO, trigger, reminder, report |
