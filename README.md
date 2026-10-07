# flow

A template for a **manage repo**: the Git-synced memory of an AI-assisted project. Your agent (Claude
Code, Codex, or any agent that reads `AGENTS.md`) does the work through it, in **experts** (one area
each, under `experts/`) and **coordinations** (work across areas, under `coordinations/`). Every request
is kept word for word, every result is a file, and every round is committed, so any new session can
pick up where the last one stopped. There are no scripts: the agent follows the markdown in `__Manager/`.

## Start a project

1. **Copy this template** into a new repo, one copy per project: use GitHub's *Use this template*, or
   `git clone https://github.com/FarhadGhE/flow my-project-manage`.
2. **Open your agent** in that folder, with your strongest model.
3. **Say `Set up this project.`** It asks four questions: the project's name, every involved directory
   (code repos, document folders, planned new code), the project in your own words, and the remote for
   this manage repo.
4. **Review the proposal.** The agent reads each directory without changing it, then proposes the first
   experts, what each one owns, and the project's settings. Say `approve`, or tell it what to change.
5. **Done.** It creates the experts, installs the commands on this machine (if you said yes), commits,
   and replaces this README with a manual for your project.

You can also let the agent do steps 1–3. Open it anywhere and say:
`Set up a manage repo for my project in ~/work/my-project-manage, using https://github.com/FarhadGhE/flow.`

**Agents:** to set up a project from this repo, clone it into the folder the user names (create the
folder if needed; it must be empty), then read `__Manager/AGENTS.md` there and follow the setup.

## Use it afterwards

| To | Say (in Codex, `$` instead of `/`) |
|---|---|
| Work in one area | `/<prefix>-plan <Expert> <request>` |
| Let the agent pick the area | `/<prefix>-plan <request>` |
| Continue in a new session | the resume line at the end of the agent's last reply |
| Work across areas | `/<prefix>-coordinate <Name> <request>`. You say `go` on a short plan before anything changes. Simple work the agent does itself; the rest it dispatches to the experts |
| Add a repo or folder | `Add <path> to the project.` |
| Set up another machine or teammate | open the agent in their clone and say `Set me up on this machine.` |
| Get a newer flow | `Upgrade the flow.` |

`<prefix>` is chosen at setup (for example `acme`, giving `/acme-plan`). Without the commands, open the
agent inside the manage repo and say `plan <Expert>: <request>` or `coordinate <Name>: <request>`.

## What is inside

`__Manager/AGENTS.md` is the first file every agent reads. `__Manager/process/` holds the rules,
procedures and templates. Projects never edit it; `Upgrade the flow.` replaces it.

## Suggestions and license

Suggestions are welcome as issues or pull requests, as long as they make sense for every project, not
just one stack or team. The flow is dedicated to the public domain under [CC0 1.0](LICENSE). You can
use, copy, modify and share it for any purpose, without asking and without credit. Contributions are
accepted under the same terms.
