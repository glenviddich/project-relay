# First-start prompt (master)

Paste this into the master project once. Replace `<OWNER>/<RELAY_REPO>`. Step 0 asks the
master to add the relay repo to its own project; if that fails, add it by hand under
**Project settings → Repositories** and paste again.

```text
Set up a task relay with the GitHub repo <OWNER>/<RELAY_REPO>, created from the
project-relay template. You are the master project.

0. Add <OWNER>/<RELAY_REPO> to this project's repositories (project settings), with push
   access. I ask for this explicitly. If you cannot, stop and tell me to add it by hand
   under Project settings → Repositories.
1. Read README.md, docs/todo-format.md, docs/patterns.md, prompts/master.md,
   prompts/worker-setup.md, prompts/courier.md and prompts/reporter.md in that repo.
2. Ask me, in ONE message, and wait for my answer:
   a. Which of my projects should take part. For each one I give you a short name
      (folder name: lowercase, digits, "-") and one sentence on what it is for.
   b. How results should come back to you (feedback mode):
      - push: a project fires a small "reporter" task in this project as soon as it
        finishes a task. Fastest; costs one cheap run per result. (Recommended.)
      - poll: the reporter runs on a timer here and looks for new results. Ask me the
        interval (e.g. every 30 min). Costs one cheap run per interval, even when idle.
      - none: no reporter. Results are only picked up when I wake you myself.
3. For each project:
   - create projects/<name>/TODO.md from templates/TODO.md (replace <name> and the date),
   - add a row to TRIGGERS.md (courier "not registered yet", plus the one-line purpose).
   Remove projects/example-blog/ only if I say so; otherwise leave it as reference.
   Commit "feat: add relay projects", push to main.
4. Feedback channel:
   - push or poll: create a scheduled task in THIS project named "relay reporter", one new
     thread per run, repo <OWNER>/<RELAY_REPO>, the prompt from prompts/reporter.md with
     the relay repo filled in, the smallest available model. push: disable it (fired on
     demand). poll: enable it with the interval I gave. I approve creating it.
   - none: create nothing.
   Fill in "## Feedback channel" in TRIGGERS.md (mode, reporter ID, interval) and the
   "## Feedback" block in every projects/<name>/TODO.md (mode, reporter ID).
   Commit "feat: set feedback mode <mode>", push.
5. Save the rules from prompts/master.md in your memory, with the relay repo filled in.
6. For each project, print the prompt from prompts/worker-setup.md with <NAME> and
   <OWNER>/<RELAY_REPO> filled in, each in its own code block, with one line:
   "Paste this into the chat of project X. It adds the relay repo itself; if not, add it
   under Project settings → Repositories and paste again."
7. Tell me: once the workers confirm, say "collect couriers". You then copy each courier ID
   from the Registration blocks into TRIGGERS.md and fire each courier once, so TODO-001
   travels through the whole loop.

Do not put any secret into the repo. If I give you one, refuse and tell me where it
belongs instead (SECURITY.md).
```

## What happens next

| Step | Where | Result |
|---|---|---|
| Worker setup pasted | each worker chat | worker creates a disabled courier task, registers its ID, fires it once as a test |
| “collect couriers” | master chat | `TRIGGERS.md` filled, each courier fired once, `TODO-001` comes back `done` |
| Normal use | master chat | “blog: …” → TODO, courier, project coordinator, result, then back to you by push, poll or your next message |
