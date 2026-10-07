---
name: {{PREFIX}}-coordinate
description: {{PROJECT}} — coordinate work across several expert areas of the project's manage repo, from any directory. Each round shows a short summary and acts after the user's go, in simple mode (the coordinator does the work itself with the experts' context) or dispatch mode (it dispatches expert sub-agents, verifies their handoffs and never codes), then commits the round. "${{PREFIX}}-coordinate Name request" continues or creates that coordination; "${{PREFIX}}-coordinate request" finds it through coordinations/index.md. Not for other projects.
---

# {{PREFIX}}-coordinate — Codex

The manage repo of {{PROJECT}} on this machine is `{{ROOT}}`.

Read `{{ROOT}}/__Manager/process/rules.md`, then `{{ROOT}}/__Manager/process/coordinate.md`, and follow
them exactly. Keep the user's original wording for the records.

How the procedure maps onto Codex:

- Ask the user with the question tool, or in chat. Never code or dispatch before the user's explicit go
  on the round's summary and authority block.
- **Dispatch** (dispatch mode) means spawning the custom agent for the command's level, with the
  envelope as its task:
  `{{PREFIX}}_expert_xhigh` (the default), `{{PREFIX}}_expert_high`, or `{{PREFIX}}_expert_medium` and
  `{{PREFIX}}_expert_low` (narrow follow-ups only). Never use a weaker model. A follow-up for the same
  expert goes to that agent's thread as a new command, with a new stamp and a new turn folder.
- Sub-agents inherit this session's sandbox and permissions, so the authority block is the real limit.
  When dispatch is impossible, give the user the envelope to paste into `${{PREFIX}}-plan`.
- Shell: absolute paths, one self-contained command per call. In PowerShell, read `.env` values as data.
- Resume lines use `${{PREFIX}}-coordinate`.

Keep output concise and proportionate; expand only when asked. Records stay complete, and user input
stays verbatim.
