# Master rules

Standing rules for the master project. [`setup.md`](setup.md) stores them in the master's
memory; `CLAUDE.md` in the repo root points Claude Code sessions here as well.

```text
RELAY: HANDING TASKS TO WORKER PROJECTS
Relay repo: <OWNER>/<RELAY_REPO>, branch main. Workers and their courier IDs: TRIGGERS.md.
Feedback mode (push / poll / none) and reporter ID: "## Feedback channel" in TRIGGERS.md.

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

2a. SHARED SOURCES. Before handing out research, check "## Shared sources" in TRIGGERS.md.
   If several projects need the same input (daily news, competitor prices, …), let ONE
   producer project write it once to shared/<topic>/<date>.md (name that exact path in
   the producer's task Limits), and give consumers tasks that name the file
   ("use shared/news/2026-01-22.md, read-only"). Hand out consumer tasks only after the
   producer's task is done. Register new shared sources in TRIGGERS.md.

3. FIRE THE COURIER. fire_trigger with the courier ID from TRIGGERS.md, without extra text
   (extra text may be refused). Disabled triggers can still be fired. The courier does not
   work on the task: it marks it "in progress", adds "Handed to project coordinator" and
   messages that project's coordinator. If the ID is missing or the call fails, tell the
   owner and offer: "paste 'please take TODO-NNN from the relay' into the project's chat".

4. COLLECT. How results reach you depends on the feedback mode in TRIGGERS.md:
   - push: the worker fires the reporter, the reporter messages you. No reminder needed.
   - poll: the reporter runs on its interval and messages you. No reminder needed.
   - none: nothing wakes you. Results are read when the owner talks to you: at the start
     of every conversation, git pull and check all tasks you handed out. Do NOT set
     reminders unless the owner asks for one.
   When woken by the reporter (or by the owner), git pull and read each named section.
   - done: report to the owner in 1–3 sentences with links. Take times from the
     result's "Times (verified)" line; if it is missing, use commit timestamps
     (git log) or say "time unknown". Never use file mtimes in your clone (they only
     show your last pull) and never estimate. Your own message time comes from `date`.
   - blocked: pass the question on. When the owner answers, add
     "- **Answer (YYYY-MM-DD):** …" below the result, set the status back to "open",
     commit, push, fire the courier again.
   - "Handoff to project coordinator failed": tell the owner to nudge the project chat.
   - still "open": the courier did not run; fire again once, then tell the owner.
   - "in progress" with a handoff line: the coordinator has it. Do NOT fire again. If it
     is still like that about 2 hours after the handoff line when you next look, tell
     the owner.
   Also check branch relay/<name> in case the worker could not push to main.

5. REGISTRY. When a worker registers or replaces its courier, copy the ID from its
   "## Registration" block into TRIGGERS.md. Keep old IDs in the "old (disabled)" column.
   Workers never edit TRIGGERS.md; the reporter only updates "Last reported commit".

6. CHANGING HOW WORKERS RUN. You cannot edit a worker's scheduled task. To change its
   courier prompt, update prompts/courier.md, then give every worker a migration task
   (docs/patterns.md, "Migration task"): create the new task, disable the old one (never
   delete), register the new ID. The owner has to nudge it once in each project chat.

7. TEMPORARY RULES. A rule for all projects with an end date (e.g. "CI quota used up, test
   locally until YYYY-MM-DD"): add a notice at the top of each TODO.md AND a task per
   project to store it in memory with the end date. Remove the notices after the date.

8. FEEDBACK MODE. If the owner changes it: update "## Feedback channel" in TRIGGERS.md and
   the "## Feedback" block in every projects/*/TODO.md (one commit), create, enable or
   disable the reporter task in this project as needed (never delete it), and tell the
   owner what changed.

9. AUTOMATED RUNS. Your own scheduled tasks (e.g. a weekly handoff) do only their fixed
   routine and read courier IDs from TRIGGERS.md at run time, never from their prompt.
   Anything beyond the routine goes to you (the master chat), not into a new thread.

10. TIMES. Every time you report or write (task dates, answers, notices) comes from a
   commit, log or chat timestamp, or from `date` for your own action. If a worker's
   result lacks a "Times (verified)" line, ask for it in the next task instead of
   reconstructing it.

11. BOUNDARIES.
   - You add task sections, answers, notices and rows in TRIGGERS.md. You never overwrite
     a worker's result.
   - Approval for guarded actions (delete, pay, send outside, change access) must come
     from the owner in the worker's own chat. Say so when passing such a question on.
   - Never delete a scheduled task without the owner's explicit word; disable it.
   - No force push, no history rewrite.
```
