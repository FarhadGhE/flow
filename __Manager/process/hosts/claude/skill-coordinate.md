---
name: {{PREFIX}}-coordinate
description: {{PROJECT}} — coordinate work across several expert areas of the project's manage repo, from any directory. The coordinator never codes. Per round it writes a plan and a plain summary, dispatches expert agents after the user approves, verifies their handoffs and commits the round. "/{{PREFIX}}-coordinate Name request" continues or creates that coordination; "/{{PREFIX}}-coordinate request" finds it through index.md. Work in one area belongs to /{{PREFIX}}-plan.
---

The manage repo of {{PROJECT}} on this machine is `{{ROOT}}`.

Read `{{ROOT}}/__Manager/process/rules.md`, then `{{ROOT}}/__Manager/process/coordinate.md`, and follow
them exactly for this request:

$ARGUMENTS

In Claude Code:

- ask the user with AskUserQuestion;
- dispatch with the Agent tool (`subagent_type` `{{PREFIX}}-expert-xhigh`, `-high` or `-max`), running
  agents in the background;
- send follow-ups with SendMessage;
- resume lines use `/{{PREFIX}}-coordinate`.
