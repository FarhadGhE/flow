---
name: {{PREFIX}}-coordinate
description: {{PROJECT}} — coordinate work across several expert areas of the project's manage repo, from any directory. Each round shows a short summary and acts after the user's go, in simple mode (the coordinator does the work itself with the experts' context) or dispatch mode (it dispatches expert agents, verifies their handoffs and never codes), then commits the round. "/{{PREFIX}}-coordinate Name request" continues or creates that coordination; "/{{PREFIX}}-coordinate request" finds it through coordinations/index.md. Work in one area belongs to /{{PREFIX}}-plan.
---

The manage repo of {{PROJECT}} on this machine is `{{ROOT}}`.

Read `{{ROOT}}/__Manager/process/rules.md`, then `{{ROOT}}/__Manager/process/coordinate.md`, and follow
them exactly for this request:

$ARGUMENTS

In Claude Code:

- ask the user with AskUserQuestion;
- in dispatch mode, dispatch with the Agent tool (`subagent_type` `{{PREFIX}}-expert-xhigh`, `-high` or
  `-max`), running agents in the background, and send follow-ups with SendMessage;
- resume lines use `/{{PREFIX}}-coordinate`.
