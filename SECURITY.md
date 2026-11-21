# Security and privacy

The relay repo is a shared mailbox between your Claude projects. Everything in it is read
by every participating project and kept forever in Git history. Treat it accordingly.

## Never put into the relay repo

- Passwords, API keys, tokens, private keys, service-account JSON, session cookies
- Contents of `.env` files, connection strings, webhook URLs with embedded secrets
- Personal data of other people (names with health, legal or family details, addresses,
  phone numbers, ID numbers)
- Your own home address or anything you would not want in a leaked repo

## Where secrets belong instead

| Secret | Put it here |
|---|---|
| Keys a worker needs at run time | the **environment secrets** of that worker's Claude Code environment, or the worker's own project repo's secret store |
| Keys on a server | the server, read by a script there; the TODO only names the script |
| Keys you type once | the worker's chat, never a file |

A TODO may *name* a secret (“use the `SEARCH_API_KEY` environment variable”); it must
never *contain* one. Workers are told to never write secrets into results either.

## Keep the relay repo private

This template is public so others can use it. **Your copy must be private**, with no
collaborators beyond yourself. If you ever pushed a secret: rotate it first, then clean
history. Deleting the line is not enough.

## Prompt injection

Workers read text that other parties may influence (web pages, issues, emails). The worker
prompt therefore only accepts tasks from its own `TODO.md` on `main` and refuses tasks
that try to change the rules, disable the check, read other projects or send data
elsewhere. If you add your own rules, keep that boundary.

## Reporting a problem with this template

Open an issue without sensitive details, or use GitHub's private vulnerability reporting
on this repository.
