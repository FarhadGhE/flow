# Upgrade — take a newer version of the flow

A direct turn in `__Manager`. It replaces `__Manager/AGENTS.md` and `__Manager/process/`, then brings the
project's own files in line with them after the user approves.

1. **Source.** Use the remote `flow` (`git remote get-url flow`). If it is missing, ask for the
   template's URL or local path and run `git remote add flow <url>`. Then run `git fetch flow`.
2. **Compare.** Check `git show flow/<branch>:__Manager/process/VERSION` against
   `__Manager/process/VERSION`, where `<branch>` is the template's default branch (usually `main`). If
   the versions are the same, say so and stop.
3. **Show** the user both versions and the size of the change
   (`git diff --stat HEAD flow/<branch> -- __Manager/AGENTS.md __Manager/process`). Local edits in
   those paths will be lost: list them and ask before continuing.
4. **Replace.** Run
   `git restore --source=flow/<branch> --staged --worktree -- __Manager/AGENTS.md __Manager/process`.
   Files that the new version does not have are removed too.
5. **Align the project's files.** Read the new `__Manager/AGENTS.md`, `process/rules.md`, the
   procedures and `process/templates/`. Compare the project's own files with what they describe:
   - the root `AGENTS.md`, `CLAUDE.md`, `README.md`, `index.md`, `.env.example`, `.gitattributes` and
     `.gitignore`;
   - `experts/roster.md`, and the shape of each expert directory;
   - `coordinations/`, its folder names and `coordinations/index.md`.

   Write `upgrade-proposal-v1.md` in this turn folder: every change needed so that the project matches
   the procedures and templates. Keep the project's content (names, paths, settings, rules, rows) and
   the text of every record. Apply it after the user approves; a change request produces the next
   version.
6. **Re-install** the per-machine commands if `__Manager/process/hosts/` changed
   (`git diff --cached --stat -- __Manager/process/hosts` lists it; `setup.md` step 8).
7. **Commit** the replaced paths and the aligned files with the message
   `__Manager: flow <old> → <new>`, adding ` — re-install the commands` when step 6 applied, then push.
   Other machines get the new version with `git pull`, and re-install the commands when the upgrade
   commit says so.
