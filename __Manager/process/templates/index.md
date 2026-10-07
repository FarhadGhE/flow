# index — routing

This is the only file a session reads to pick a directory; coordinations have their own index,
`coordinations/index.md`. Never scan the tree. Rows carry no status. They change only when a directory
is created, closed or rescoped. Status lives in each directory: in `important.md`, `session-index.md`
and a turn's `session-summary.md`; for coordinations, in `coordinations/index.md` and each
coordination's `session.md`.

## Experts

| Directory | What it holds | Continue here when the task is about… |
|---|---|---|
| `experts/{{NAME}}` | {{WHAT IT HOLDS}} | {{WHEN TO CONTINUE HERE}} |
| `__Manager` | The flow and this project's process records | Process changes, setup, adding directories, upgrades. Never coordinated |

## Coordinations

Work across several experts runs in `coordinations/<stamp>-<Name>/`, through `/{{PREFIX}}-coordinate`.
`coordinations/index.md` lists every coordination, newest first. Owned paths are in
`experts/roster.md`.

## Known upcoming topics — no directory yet

- {{TOPIC}}: {{WHY IT IS EXPECTED}}
