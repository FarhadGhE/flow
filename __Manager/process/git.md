# Git — the exact commands

`<root>` is the manage repo's absolute path and `<branch>` its branch (root `AGENTS.md`). The commands
are POSIX. In PowerShell, use the same Git commands, starting with `Set-Location <abs>;` instead of
`cd <abs> &&`.

## Audit: the start of every session, before any edit

```sh
cd "<root>" && git status --porcelain && git fetch --quiet origin && git status -sb | head -n 1
```

- **Uncommitted files in your own turn folders** (`*-direct-<FLOW_DEVELOPER>`, or the coordination you
  run): tell the user, then finish or commit them first, with their OK.
- **Uncommitted files from anyone else**: leave them alone. Never commit, stash, reset, restore or clean
  them.
- **`behind`**: run `cd "<root>" && git merge --ff-only origin/<branch>`. If it refuses because of local
  changes, report that and continue without merging.
- **`ahead`**: push (see below) before new work.
- **No remote**: skip the fetch and the push. The receipt says "not pushed — no remote".

## Stamp

`<stamp>` is `yyyy-MM-dd_HH-mm` in the project's time zone (`<zone>`, from the root `AGENTS.md`):

```sh
TZ=<zone> date +%Y-%m-%d_%H-%M
```

In PowerShell:
`[TimeZoneInfo]::ConvertTimeBySystemTimeZoneId([DateTime]::UtcNow, '<Windows zone id>').ToString('yyyy-MM-dd_HH-mm')`.
If the turn or coordination folder already exists, take the next minute.

## Commit only your own paths

```sh
cd "<root>" && git add -A -- <path> [<path> …] && git commit -m "<Dir>: <what this round did>" -- <path> [<path> …]
```

- **Your paths**: your turn folder, plus only the shared files you edited — that directory's
  `session-index.md` and `important.md`, and any root files the task told you to change. A coordinator's
  paths are the coordination folder, `coordinations/index.md`, and the round's verified turn folders
  with those experts' `session-index.md` and the `important.md` files the round edited.
- `-A` with paths also stages your own moves and deletions inside those paths.
- Never use `git add .`, `git add -A` without paths, `git commit -a`, `git stash`, `git reset --hard`,
  `git clean` or `git checkout -- .`.
- Before committing an `important.md`, check `wc -c <file>`: it must be at most 30720.
- Commit messages: `experts/<Name>: …`, `__Manager: …`, `coordinations/<stamp>-<Name>: round NN — …`.

## Push and verify

```sh
cd "<root>" && git push origin HEAD:<branch>
```

If the push is rejected because the remote moved on:

```sh
cd "<root>" && git fetch origin && git merge --no-edit origin/<branch> && git push origin HEAD:<branch>
```

- **A conflict in a `session-index.md` or in `coordinations/index.md`**: keep every row. If your turn
  or coordination appears twice, keep the row that matches your `session-summary.md` or `session.md`.
- **Any other conflict**: run `git merge --abort`, stop, and report **BLOCKED** with the paths. Never
  resolve another session's content.
- **Verify**: `git rev-parse HEAD` must equal the first field of `git ls-remote origin refs/heads/<branch>`.

## Receipt line: the last line of every reply that ends a round

- `Manage: <short sha> pushed`
- `Manage: <short sha> committed, not pushed — <reason>`
- `Manage: round NN open — last checkpoint <short sha | none>`
- `BLOCKED: <exact reason>`

## Code repos

Code changes happen only after the user's go (`rules.md` §5): in an expert's direct turn, in a
simple-mode round, or in a coordinated turn of a dispatch-mode round.

- **Branch.** Before any change, the session that starts the work (the direct turn, or the coordinator
  in either mode) makes sure each code repo it involves is clean (`git -C <repo> status --porcelain`
  prints nothing; otherwise it stops and asks), then creates the feature branch:
  `git -C <repo> fetch origin && git -C <repo> switch --no-track -c <feature-prefix><kebab-name> origin/<integration-branch>`.
  - `<kebab-name>` comes from the coordination's `<Name>` or from the change plan. If that branch
    already exists, add the date: `<kebab-name>-<yyyy-MM-dd>`.
  - A later round continues on it: `git -C <repo> switch <feature-prefix><kebab-name>`. After a merge,
    cut `<feature-prefix><kebab-name>-rNN`.
- **Commit.** Once per expert area in each repo, with only that area's changed paths:
  `git -C <repo> add -A -- <paths> && git -C <repo> commit -m "<Expert>: <what> [<suffix>]" -- <paths>`.
  `<suffix>` is `<Name> rNN Cn` in a coordinated turn, `<Name> rNN` in a simple-mode round, and
  `direct <stamp>` in a direct turn. Shared host files go in the commit of the area that needed them.
- **Locks.** If `index.lock` exists, or another build holds files, wait 10 seconds and retry, up to 5
  times. Never delete a lock while a Git process is running, and never delete another expert's build
  output.
- Never push, merge, rebase, reset, stash or clean in a code repo unless the approved authority says so.
  Only the session that starts the work creates or switches branches; an expert in a coordinated turn
  never does.
