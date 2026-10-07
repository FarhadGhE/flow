# The flow — read me first

This repository is (or is about to become) a **manage repo**: the shared, Git-synced memory of one
project. Agents do the project's work through it, in **experts** (`experts/<Name>/`, one area each) and
**coordinations** (`coordinations/<stamp>-<Name>/`, work across areas, listed in
`coordinations/index.md`). Everything is recorded in files and committed. Nothing lives only in chat or
in an agent's memory.

## Which procedure applies now

Read `process/rules.md` first (it is short), then the procedure that matches:

| Situation | Read and follow |
|---|---|
| No `index.md` at the repo root yet, and the user wants to start | `process/setup.md` |
| The repo is set up, but this machine has no `.env` | `process/join.md`, then the user's request |
| The user wants to add a directory or repo to the project | `process/add.md` |
| The user wants a newer version of the flow | `process/upgrade.md` |
| Work in one area: `<prefix>-plan …`, or "plan <Expert>: …" | `process/turn.md` |
| Work across areas: `<prefix>-coordinate …`, or "coordinate <Name>: …" | `process/coordinate.md` |
| A message that starts with `FLOW COMMAND` | `process/turn.md`, coordinated turn |
| Anything else in a set-up repo | `process/turn.md`; it routes through `index.md` |

## Never

- edit `__Manager/AGENTS.md` or anything in `__Manager/process/` inside a project. An upgrade replaces
  them;
- shorten, correct or reword the user's words where the flow keeps them verbatim;
- change a code repo before the user's go on a written plan, or commit, push, merge or open a PR there
  beyond what that plan's authority allows;
- stage without paths (`git add .`, or `git add -A` with no paths), or commit files your session did
  not write;
- create a remote, push to a new remote, install anything outside this repo, or contact anyone without
  the user's yes.

## Map

| File | What it is |
|---|---|
| `process/rules.md` | the rules for every session |
| `process/style.md` | how deliverables and replies look |
| `process/git.md` | the exact Git commands (no scripts) |
| `process/templates/` | the shape of every file setup creates |
| `process/hosts/` | per-machine commands and expert agents for Claude Code and Codex |
| `process/VERSION` | the flow's version |
| `process/LICENSE` | the flow's license: CC0 1.0, public domain |
