# {{PROJECT}} — manage repo

This is the shared memory of {{PROJECT}}. AI agents work here in **experts** (`experts/`, one area each)
and **coordinations** (`coordinations/`, work across areas). Every request is kept word for word and
every round is committed, so any session can continue where the last one stopped.

## Use it

| To | Say (in Codex, `$` instead of `/`) |
|---|---|
| Work in one area | `/{{PREFIX}}-plan <Expert> <request>`, for example `/{{PREFIX}}-plan {{EXAMPLE_EXPERT}} {{EXAMPLE_REQUEST}}` |
| Let the agent pick the area | `/{{PREFIX}}-plan <request>` |
| Continue in a new session | the resume line at the end of the agent's last reply |
| Work across areas | `/{{PREFIX}}-coordinate <Name> <request>`, for example `/{{PREFIX}}-coordinate {{EXAMPLE_COORDINATION}} {{EXAMPLE_COORDINATION_REQUEST}}` |
| Get a plan for another agent to execute | ask for an "implementation plan" in a turn |
| Add a repo or folder | `Add <path> to the project.` |
| Get a newer flow | `Upgrade the flow.` |

Without the per-machine commands, open the agent inside this repo and say `plan <Expert>: <request>`
or `coordinate <Name>: <request>`.

## Experts

| Expert | Owns |
|---|---|
| `{{NAME}}` | {{WHAT IT OWNS, IN A FEW WORDS}} |

Routing is in `index.md`, owned paths in `experts/roster.md`, and settings and authority defaults in
`AGENTS.md`.

## Good to know

- Every round ends with a line like `Manage: 3f2c1a9 pushed`. A reply without it is unfinished, so ask
  for it.
- In a coordination you first approve a short summary: who does what, the final state, and the
  authority (branch, push, PR, migrations, deploys, spending). Experts code on a feature branch and
  commit locally. The coordinator verifies their work and never codes. Pushes and PRs happen only as
  approved.
- Several people can work in different experts, or the same one, at the same time. Only one person
  works on a coordination at a time.

## Another machine or teammate

Clone this repo, open the agent in it, and say `Set me up on this machine.`
