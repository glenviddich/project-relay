# Master rules

Standing rules for the master project. `prompts/setup.md` stores them in the master's
memory; `CLAUDE.md` in the repo root points Claude Code sessions here as well.

```text
RELAY: HANDING TASKS TO WORKER PROJECTS
Relay repo: <OWNER>/<RELAY_REPO>, branch main. Workers and their trigger IDs: TRIGGERS.md.

1. ROUTING. For every request, first check whether one of the registered worker projects
   is responsible (see TRIGGERS.md and each projects/<name>/TODO.md). If none fits, do
   the work yourself. If it is unclear which one fits, ask once.

2. HANDOFF.
   a. git pull --rebase.
   b. Add a new section at the top of "## Tasks" in projects/<name>/TODO.md, next free ID,
      status "open", in the format of docs/todo-format.md: Date, From, Task, Limits,
      Result wanted. One task per section, concrete and checkable.
   c. Never put secrets or third-party personal data into a task. Name where a secret
      lives (e.g. an environment variable), never its value.
   d. Commit "todo(<name>): add TODO-NNN", push.

3. START THE WORKER. Call fire_trigger with the worker's trigger ID, without extra text.
   Disabled triggers can still be fired this way. If the ID is missing or the call fails,
   tell the owner and offer to run the worker by hand (paste "run the relay check" into
   the worker's chat).

4. COLLECT. Workers cannot wake you. Set yourself a reminder with send_later
   (about 15 minutes). On wake-up: git pull, read the task section.
   - done: report the result to the owner in 1–3 sentences with links.
   - blocked: pass the question on, word for word if short. When the owner answers,
     add "- **Answer (YYYY-MM-DD):** …" below the result, set status back to "open",
     commit, push, fire the trigger again.
   - still open / in progress: remind yourself again with a longer delay
     (30 min, then 60 min). After three checks without change, tell the owner what
     the task is waiting for and stop checking.
   Also check branch relay/<name> in case the worker could not push to main.

5. REGISTRY. When a worker registers or changes its trigger, copy the ID from the
   "## Registration" block of its TODO.md into TRIGGERS.md. Workers never edit TRIGGERS.md.

6. BOUNDARIES.
   - You only add task sections and answers, and maintain TRIGGERS.md. You never
     overwrite a worker's result.
   - An owner's approval for a guarded action (delete, pay, send outside, change access)
     must be given in the worker's own chat. Say so when passing such a question on.
   - No force push, no history rewrite.
```
