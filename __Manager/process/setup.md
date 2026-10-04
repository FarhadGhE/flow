# Setup — make a fresh template copy this project's manage repo

Setup runs once, in a fresh copy (no `index.md` at the root yet). It is one direct turn in `__Manager`
with two rounds:

- **Round 1** asks, discovers and proposes. Nothing outside the setup turn is created before the user
  approves.
- **Round 2** builds.

Read `rules.md` first. Use the strongest model available, because this is the longest procedure in the
flow.

## Round 1 — ask, discover, propose

### 1. Ask four questions, in one message

1. **Name.** The project's name.
2. **Directories.** Every involved directory: its absolute path; its kind (code, documents, other); and
   whether it exists or is planned (new code that does not exist yet). For a code folder that is not a
   clone yet, its Git URL.
3. **Description.** The project in the user's own words: what it is, who it is for, what matters now.
4. **Remote.** Show what `git remote -v` prints. If `origin` is the flow template this copy came from, it
   will become the remote `flow`, kept for upgrades; ask for this manage repo's own remote URL, or
   "none yet". If `origin` is already the project's own repo, confirm that.

Ask nothing else now. Every other choice becomes a default in the proposal.

### 2. Prepare

- **Not a Git repo yet**: run `git init -b main`, then commit the template files with explicit paths:
  `git add -A -- README.md AGENTS.md CLAUDE.md .gitignore .gitattributes __Manager` and
  `git commit -m "flow template <VERSION>" -- README.md AGENTS.md CLAUDE.md .gitignore .gitattributes __Manager`.
- **Remotes, as answered**: if `origin` is the template, run `git remote rename origin flow`. Add the
  project's own remote as `origin` only in round 2, after approval.
- **Defaults to collect on this machine**:
  - developer handle: the letters and digits of `git config user.name`;
  - time zone: on macOS `readlink /etc/localtime`, on Linux `timedatectl show -p Timezone --value` or
    `/etc/timezone`, on Windows `tzutil /g`;
  - manage branch: `git branch --show-current`;
  - hosts: `~/.claude/` means Claude Code, `~/.codex/` means Codex.
- **`.env`**: write it from `process/templates/env.example` with this machine's values. It is never
  committed.
- **The setup turn**: stamp (`git.md`) and create `__Manager/turns/turn-<stamp>-direct-<handle>/` with:
  - `feedbacks.md`: the setup request and the four answers, verbatim, latest on top (template in
    `turn.md` §3);
  - `session-summary.md` with `Status: IN PROGRESS` (template in `turn.md` §3).
- **`__Manager/task.txt`**: the user's setup message, verbatim.

### 3. Discover every directory: read-only and bounded

In the involved directories:

- never write anything;
- never run their build, install or tests;
- never open `.env*` files, keys, certificates or secret files.

Read structure, not every file.

For a Git directory, look at these, in this order:

1. **Size and layout**: where the code is.
2. **Manifests**: modules and stack. Look for `package.json`, `pnpm-workspace.yaml`, `nx.json`,
   `project.json`, `turbo.json`, `*.sln`, `*.slnx`, `*.csproj`, `go.mod`, `go.work`, `Cargo.toml`,
   `pyproject.toml`, `requirements*.txt`, `pom.xml`, `build.gradle*`, `settings.gradle*`, `Gemfile`,
   `composer.json`.
3. **Existing rules and ownership**: boundaries, owners and rules already written down. Look for
   `README*`, `CONTRIBUTING*`, `ARCHITECTURE*`, `CODEOWNERS`, `AGENTS.md`, `CLAUDE.md`, `.cursorrules`,
   `.github/copilot-instructions.md`, ADR folders.
4. **Feature folders**: `Modules/*`, `modules/*`, `features/*`, `apps/*`, `packages/*`, `services/*`,
   and the framework's own app folders.
5. **Layers**: migrations, ORM schema, Terraform, Bicep, Helm, k8s, Dockerfiles, CI workflows, OpenAPI,
   `.proto` files, contracts, the frontend framework.
6. **Activity**: live areas, areas that change together, and the people involved.
7. **Composition roots**: `Program.cs`, `main.ts`, `app.module.ts`, root manifests, lockfiles and CI
   files. These become shared host files.
8. **Build and test commands**: package scripts, Makefiles, test projects, CI steps. They go into each
   expert's Scope.

Commands for 1 and 6 (adapt them to the shell):

```sh
git -C <dir> ls-files | wc -l
git -C <dir> ls-files | cut -d/ -f1-2 | sort | uniq -c | sort -rn | head -n 60
git -C <dir> log --since=6.months --format= --name-only | cut -d/ -f1-2 | sort | uniq -c | sort -rn | head -n 30
git -C <dir> shortlog -sn --since=6.months HEAD | head -n 10
git -C <dir> log -n 60 --name-only --format='--- %h %s'
```

- **A documents directory**: list it (`find <dir> -maxdepth 3 -type f | head -n 200`) and read titles
  and headings.
- **A planned directory**: there is nothing to read; the description is the source.
- **Large directories** (thousands of files, or many sub-projects): if the host can run read-only
  sub-agents, give each one a single directory, with this section as its task and
  `templates/discovery.md` as the shape of its answer. Otherwise, finish one directory's report before
  starting the next.

Write one `discovery-<name>-v1.md` per directory in the setup turn folder, from
`process/templates/discovery.md`. It contains facts only, each with its source (a file, a manifest, or a
command). Refer to directories by their `.env` key, never by an absolute path.

### 4. Propose

Write `setup-proposal-v1.md` in the setup turn folder, from `process/templates/setup-proposal.md`.

**How to cut the experts:**

- **Feature experts** for features that span layers. A module's API, application and data folders
  belong to one expert.
- **Layer experts** only for layers the stack actually has and that several features share: database
  and migrations, frontend, infrastructure and CI, contracts between components.
- **`Architecture`** always, for cross-cutting decisions and ADRs. It owns no code paths unless an ADR
  folder exists.
- **Non-code experts** (contracts, costs, client communication, research) only when a documents
  directory or the description calls for them.
- **As few as the project needs**: usually 3–12; a small app may need only 2–4. Each expert needs a
  one-sentence domain, plus either owned paths or a clear non-code remit. Anything thinner becomes a
  known upcoming topic in `index.md`, not an empty directory.
- **Owned paths are disjoint**, written as a repo key plus globs. Composition roots, root manifests,
  lockfiles and tests shared by several features are shared host files with no default owner.
- **Planned code**: `Architecture` plus experts from the description, with *planned* paths. Layer
  experts follow once a decision fixes the stack.
- **Names** are PascalCase and specific (`Billing`, not `Backend2`).

**Settings, each with a default:**

- **command prefix**: lowercase letters and digits from the name, not already used in
  `~/.claude/skills/`, `~/.agents/skills/` or `~/.codex/agents/`;
- **manage branch**;
- **time zone**: a zone with daylight saving gets a fixed UTC offset instead, so folder names never
  repeat or go backwards;
- **developer handle**;
- **per code repo**:
  - its key and remote;
  - the integration branch: `dev` when it exists, otherwise the default branch;
  - protected branches: the default branch plus the integration branch;
  - the feature prefix: `feature/`;
  - restricted commands found in discovery (`dotnet ef`, `prisma migrate`, `alembic upgrade`,
    `manage.py migrate`, `rails db:migrate`, `terraform apply`, `kubectl apply`, `helm upgrade`, …);
- **expert models**: Claude experts `opus` at `high`, `xhigh` (default) and `max`; Codex experts
  `gpt-6-astra` at `low`, `medium`, `high` and `xhigh` (default). Use the strongest models the user
  names instead, if they name any.

**Authority defaults:** the rows of `process/templates/AGENTS.md`, plus one row per restricted command,
deploy, database or cloud change, and paid API call found in discovery (with a budget).

**What round 2 creates and installs:** the file list; and, for each host found, the per-machine
commands and expert agents with their paths. These are installed only with a yes.

End with numbered questions, each with a recommendation, and with the line:
`Reply approve, or tell me what to change.`

### 5. Close round 1

1. Update `feedbacks.md` (`Answered by:`) and `session-summary.md`. Create
   `__Manager/session-index.md` from `process/templates/session-index.md` and write the setup turn's
   row.
2. Commit locally (`git.md`) `__Manager/task.txt`, `__Manager/session-index.md` and the setup turn
   folder.
3. Reply with:
   - the experts table;
   - the questions;
   - the receipt line;
   - this line: `To continue this setup in a new session: open the agent in this repo and say "continue the setup".`

For each change request: record it verbatim, write `setup-proposal-v<N+1>.md` (complete, standing
alone), commit, and ask again. Build nothing before the user explicitly approves one version.

## Round 2 — build, after the user approves a proposal version

6. **Record** the approval verbatim in `feedbacks.md`.
7. **Create** the files from `process/templates/`, filling every `{{placeholder}}` and repeating
   template rows as needed, from the approved proposal:
   - **root**:
     - `AGENTS.md`, which replaces the "not set up yet" file;
     - `CLAUDE.md`;
     - `README.md`, which replaces the template's manual;
     - `index.md`;
     - `.env.example`;
   - **`__Manager/important.md`**: the header only, unless setup learned something a later process
     session needs;
   - **`experts/roster.md`**;
   - **for each approved expert, `experts/<Name>/`**:
     - `task.txt` (from `templates/expert-task.txt`);
     - `important.md` (the header plus a `## Scope` from discovery, every line with its source);
     - `session-index.md` (the header).
8. **Install on this machine**, only what was approved. Fill `process/hosts/<host>/` with the prefix,
   the project name, this machine's absolute path to the repo, the model and the efforts. `{{USE}}`
   is each effort's use:
   - Claude: `high` for narrow fixes, `xhigh` the default for coded turns, `max` for the hardest
     design;
   - Codex: `low` and `medium` for narrow follow-ups only, `high` for small, well-scoped turns,
     `xhigh` the default.

   The files go here:
   - **Claude Code**:
     - `~/.claude/skills/<prefix>-plan/SKILL.md`;
     - `~/.claude/skills/<prefix>-coordinate/SKILL.md`;
     - `~/.claude/agents/<prefix>-expert-<effort>.md`, one per effort.
   - **Codex**:
     - `~/.agents/skills/<prefix>-plan/SKILL.md`;
     - `~/.agents/skills/<prefix>-coordinate/SKILL.md`;
     - `~/.codex/agents/<prefix>_expert_<effort>.toml`, one per effort (under `$CODEX_HOME` if it is
       set).

   If a target file exists and does not name this repo's path, never overwrite it: stop and ask.
9. **Verify**, and write the result into `session-summary.md`:
   - `git status --porcelain` shows exactly the planned files.
   - Nothing changed in any involved directory: `git -C <dir> status --porcelain` is the same as before
     setup.
   - Every expert has `task.txt`, `important.md` and `session-index.md`, and appears in both `index.md`
     and `experts/roster.md`.
   - Owned paths are disjoint: compare every pair of globs.
   - No file to be committed contains this machine's home path or a `.env` value:
     `grep -rn "<home path>" --exclude-dir=.git --exclude=.env .` must find nothing.
   - Every `important.md` is at most 30,720 bytes.
10. **Commit and push.**
    1. Set the setup turn's `session-summary.md` to `Status: DONE`, and its `session-index.md` row too.
    2. Commit (`git.md`) the root files, `__Manager` and `experts`.
    3. Add the approved remote as `origin`, and push with `-u`.
11. **Reply** with:
    - the experts and what each owns;
    - three first commands to try, with the real prefix and expert names;
    - "restart the agent so it loads the new commands";
    - the receipt line.
