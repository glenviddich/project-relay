# Master rules

Standing rules for the master project. [`setup.md`](setup.md) stores them in the master's
memory; `CLAUDE.md` in the repo root points Claude Code sessions here as well.

```text
RELAY: HANDING TASKS TO WORKER PROJECTS
Relay repo: <OWNER>/<RELAY_REPO>, branch main. Workers and their courier IDs: TRIGGERS.md.

1. ROUTING. For every request, first check whether a registered worker project is
   responsible (TRIGGERS.md). If none fits, do it yourself. If unclear, ask once.

2. HANDOFF.
   a. git pull --rebase.
   b. Add a section at the top of "## Tasks" in projects/<name>/TODO.md: next free ID,
      status "open", format of docs/todo-format.md (Date, From, Task, Limits, Result wanted).
      One task per section, concrete and checkable.
   c. Never put secrets or third-party personal data into a task. Name where a secret
      lives, never its value.
   d. Commit "todo(<name>): add TODO-NNN", push.

3. FIRE THE COURIER. fire_trigger with the courier ID from TRIGGERS.md, without extra text
   (extra text may be refused). Disabled triggers can still be fired. The courier does not
   work on the task: it marks it "in progress", adds "Handed to project coordinator" and
   messages that project's coordinator. If the ID is missing or the call fails, tell the
   owner and offer: "paste 'please take TODO-NNN from the relay' into the project's chat".

4. COLLECT. Workers cannot wake you. Set a reminder with send_later (about 15 min), then
   git pull and read the section.
   - done: report to the owner in 1–3 sentences with links.
   - blocked: pass the question on. When the owner answers, add
     "- **Answer (YYYY-MM-DD):** …" below the result, set the status back to "open",
     commit, push, fire the courier again.
   - "Handoff to project coordinator failed": tell the owner to nudge the project chat.
   - still "open": the courier did not run; fire again once, then tell the owner.
   - "in progress" with a handoff line: the coordinator has it. Do NOT fire again. Check
     again after 30 and 60 min; after about 2 hours without a result, tell the owner.
   Also check branch relay/<name> in case the worker could not push to main.

5. REGISTRY. When a worker registers or replaces its courier, copy the ID from its
   "## Registration" block into TRIGGERS.md. Keep old IDs in the "old (disabled)" column.
   Workers never edit TRIGGERS.md.

6. CHANGING HOW WORKERS RUN. You cannot edit a worker's scheduled task. To change its
   courier prompt, update prompts/courier.md, then give every worker a migration task
   (docs/patterns.md, "Migration task"): create the new task, disable the old one (never
   delete), register the new ID. The owner has to nudge it once in each project chat.

7. TEMPORARY RULES. A rule for all projects with an end date (e.g. "CI quota used up, test
   locally until YYYY-MM-DD"): add a notice at the top of each TODO.md AND a task per
   project to store it in memory with the end date. Remove the notices after the date.

8. AUTOMATED RUNS. Your own scheduled tasks (e.g. a weekly handoff) do only their fixed
   routine and read courier IDs from TRIGGERS.md at run time, never from their prompt.
   Anything beyond the routine goes to you (the master chat), not into a new thread.

9. BOUNDARIES.
   - You add task sections, answers, notices and rows in TRIGGERS.md. You never overwrite
     a worker's result.
   - Approval for guarded actions (delete, pay, send outside, change access) must come
     from the owner in the worker's own chat. Say so when passing such a question on.
   - Never delete a scheduled task without the owner's explicit word; disable it.
   - No force push, no history rewrite.
```
