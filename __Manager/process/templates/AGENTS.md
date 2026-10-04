# {{PROJECT}} — manage repo

This is {{PROJECT}}'s manage repo, run by the flow. For any request, read `__Manager/AGENTS.md` (which
procedure applies) and `__Manager/process/rules.md` first. Route with `index.md`. On a machine without
`.env`, run `__Manager/process/join.md` first.

## The project

{{TWO OR THREE SENTENCES FROM THE APPROVED PROPOSAL}}

## Settings

| Setting | Value |
|---|---|
| Command prefix | `{{PREFIX}}`: `/{{PREFIX}}-plan`, `/{{PREFIX}}-coordinate` (in Codex, `$` instead of `/`) |
| Manage branch | `{{BRANCH}}` |
| Turn stamps | `{{ZONE}}` |
| Claude experts | `{{CLAUDE_MODEL}}`: `{{PREFIX}}-expert-high` (narrow fixes), `-xhigh` (default), `-max` (hardest design) |
| Codex experts | `{{CODEX_MODEL}}`: `{{PREFIX}}_expert_xhigh` (default), `_high`, `_medium`, `_low` (narrow follow-ups) |
| Flow template | remote `flow`, `{{TEMPLATE_URL}}`; version in `__Manager/process/VERSION` |

## Directories

| Key (`.env`) | Kind | Remote | Integration branch | Protected | Feature prefix | Restricted commands |
|---|---|---|---|---|---|---|
| `{{KEY}}` | {{KIND}} | `{{REMOTE}}` | `{{INTEGRATION_BRANCH}}` | `{{PROTECTED}}` | `{{FEATURE_PREFIX}}` | `{{RESTRICTED}}` |

## Authority defaults

Every coordination summary starts from these rows. A tick applies to that round only.

| Item | Default |
|---|---|
| Code repos involved; branch `<feature prefix><kebab-name>` from a fresh integration branch | stated per round |
| Experts' local path-scoped commit per coordinated turn | always on |
| Push the feature branch at round close (never a protected branch) | no |
| Open a PR into the integration branch | no |
| {{ONE ROW PER RESTRICTED COMMAND, DEPLOY, DATABASE OR CLOUD CHANGE, AND PAID API CALLS WITH A BUDGET}} | no |
| Contact people or send messages | no |

## Project rules

Locked rules for the whole project, added when the user decides them. Keep this file short; area rules
belong in `experts/<Name>/important.md`.
