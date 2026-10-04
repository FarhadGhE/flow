---
name: {{PREFIX}}-plan
description: {{PROJECT}} — work in one expert area of the project's manage repo, from any directory. "/{{PREFIX}}-plan Expert request" starts a turn; "/{{PREFIX}}-plan experts/Expert/turns/turn-folder message" continues one; "/{{PREFIX}}-plan request" finds the area through index.md; a FLOW COMMAND runs one coordinated turn. Work across several areas belongs to /{{PREFIX}}-coordinate.
---

The manage repo of {{PROJECT}} on this machine is `{{ROOT}}`.

Read `{{ROOT}}/__Manager/process/rules.md`, then `{{ROOT}}/__Manager/process/turn.md`, and follow them
exactly for this request:

$ARGUMENTS

In Claude Code, ask the user with AskUserQuestion. Resume lines use `/{{PREFIX}}-plan`.
