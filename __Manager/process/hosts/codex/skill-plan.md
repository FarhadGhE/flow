---
name: {{PREFIX}}-plan
description: {{PROJECT}} — work in one expert area of the project's manage repo, from any directory. "${{PREFIX}}-plan Expert request" starts a turn; "${{PREFIX}}-plan experts/Expert/turns/turn-folder message" continues one; "${{PREFIX}}-plan request" finds the area through index.md; a FLOW COMMAND runs one coordinated turn. Work across areas belongs to ${{PREFIX}}-coordinate. Not for other projects.
---

# {{PREFIX}}-plan — Codex

The manage repo of {{PROJECT}} on this machine is `{{ROOT}}`.

Read `{{ROOT}}/__Manager/process/rules.md`, then `{{ROOT}}/__Manager/process/turn.md`, and follow them
exactly. Keep the user's original wording for the records.

How the procedure maps onto Codex:

- Ask the user with the question tool, or in chat. A coordinated turn cannot ask: its questions go in
  its handoff.
- Shell: absolute paths, one self-contained command per call. In PowerShell, read `.env` values as data.
- Resume lines use `${{PREFIX}}-plan`.
- When the sandbox blocks a Git or network operation the procedure authorizes, use the normal approval
  flow. If approval is denied, keep the work and report BLOCKED.

Keep output concise and proportionate: the essential answer, the reasoning and any material caveats;
expand only when asked. Records stay complete, and user input stays verbatim.
