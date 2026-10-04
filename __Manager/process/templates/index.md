# index — routing

This is the only file a session reads to pick a directory; never scan the tree. Rows carry no status.
They change only when a directory is created, closed or rescoped. Status lives in each directory: in
`important.md`, `session-index.md` and a turn's `session-summary.md`, or in a coordination's
`session.md`.

## Experts

| Directory | What it holds | Continue here when the task is about… |
|---|---|---|
| `experts/{{NAME}}` | {{WHAT IT HOLDS}} | {{WHEN TO CONTINUE HERE}} |
| `__Manager` | The flow and this project's process records | Process changes, setup, adding directories, upgrades. Never coordinated |

## Coordinations

Work across several experts runs in `coordinations/<Name>/`, through `/{{PREFIX}}-coordinate`. Owned
paths are in `experts/roster.md`.

| Coordination | What it coordinates | Continue here when… |
|---|---|---|

## Known upcoming topics — no directory yet

- {{TOPIC}}: {{WHY IT IS EXPECTED}}
