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

```sh
TZ=<zone> date +%Y-%m-%d_%H-%M
```

In PowerShell:
`[TimeZoneInfo]::ConvertTimeBySystemTimeZoneId([DateTime]::UtcNow, '<Windows zone id>').ToString('yyyy-MM-dd_HH-mm')`.
`<zone>` is the project's time zone, from the root `AGENTS.md`. If the turn folder already exists, take
the next minute.

## Commit only your own paths

```sh
cd "<root>" && git add -A -- <path> [<path> …] && git commit -m "<Dir>: <what this round did>" -- <path> [<path> …]
```

- **Your paths**: your turn folder, plus only the shared files you edited — that directory's
  `session-index.md` and `important.md`, and any root files the task told you to change. A coordinator's
  paths are the coordination folder and the round's verified turn folders.
- `-A` with paths also stages your own moves and deletions inside those paths.
- Never use `git add .`, `git add -A` without paths, `git commit -a`, `git stash`, `git reset --hard`,
  `git clean` or `git checkout -- .`.
- Before committing an `important.md`, check `wc -c <file>`: it must be at most 30720.
- Commit messages: `experts/<Name>: …`, `__Manager: …`, `coordinations/<Name>: round NN — …`.

## Push and verify

```sh
cd "<root>" && git push origin HEAD:<branch>
```

If the push is rejected because the remote moved on:

```sh
cd "<root>" && git fetch origin && git merge --no-edit origin/<branch> && git push origin HEAD:<branch>
```

- **A conflict in a `session-index.md`**: keep every row. If your turn appears twice, keep the row that
  matches your summary.
- **Any other conflict**: run `git merge --abort`, stop, and report **BLOCKED** with the paths. Never
  resolve another session's content.
- **Verify**: `git rev-parse HEAD` must equal the first field of `git ls-remote origin refs/heads/<branch>`.

## Receipt line: the last line of every reply that ends a round

- `Manage: <short sha> pushed`
- `Manage: <short sha> committed, not pushed — <reason>`
- `Manage: round NN open — last checkpoint <short sha | none>`
- `BLOCKED: <exact reason>`

## Code repos (coordinations only)

- **Branch.** Before dispatch, the coordinator makes sure each involved code repo is clean
  (`git -C <repo> status --porcelain` prints nothing; otherwise it stops and asks), then creates the
  feature branch:
  `git -C <repo> fetch origin && git -C <repo> switch --no-track -c <feature-prefix><kebab-name> origin/<integration-branch>`.
  A later round continues on it: `git -C <repo> switch <feature-prefix><kebab-name>`.
- **Commit.** An expert commits once per turn, and only the owned paths it changed:
  `git -C <repo> add -A -- <paths> && git -C <repo> commit -m "<Expert>: <what> [<Name> rNN Cn]" -- <paths>`.
- **Locks.** If `index.lock` exists, or another build holds files, wait 10 seconds and retry, up to 5
  times. Never delete a lock while a Git process is running, and never delete another expert's build
  output.
- Never push, merge, rebase, reset, stash, clean or switch branches in a code repo unless the round's
  authority says so.
