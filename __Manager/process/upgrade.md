# Upgrade — take a newer version of the flow

A direct turn in `__Manager`. It replaces `__Manager/AGENTS.md` and `__Manager/process/`, and changes
nothing else unless the CHANGELOG says so.

1. **Source.** Use the remote `flow` (`git remote get-url flow`). If it is missing, ask for the
   template's URL or local path and run `git remote add flow <url>`. Then run `git fetch flow`.
2. **Compare.** Check `git show flow/<branch>:__Manager/process/VERSION` against
   `__Manager/process/VERSION`, where `<branch>` is the template's default branch (usually `main`). If
   the versions are the same, say so and stop.
3. **Show** the user:
   - the CHANGELOG entries between the two versions
     (`git show flow/<branch>:__Manager/process/CHANGELOG.md`);
   - the size of the change
     (`git diff --stat HEAD flow/<branch> -- __Manager/AGENTS.md __Manager/process`).

   Local edits in those paths will be lost: list them and ask before continuing.
4. **Replace.** Run
   `git restore --source=flow/<branch> --staged --worktree -- __Manager/AGENTS.md __Manager/process`.
   Files the new version removed are removed too.
5. **Apply** each crossed version's "Project changes" from the CHANGELOG, exactly as written and
   nothing more.
6. **Re-install** the per-machine commands if `process/hosts/` changed (`setup.md` step 8).
7. **Commit** the replaced paths plus any project changes with the message
   `__Manager: flow <old> → <new>`, then push. Other machines get the new version with `git pull`. They
   re-install the commands only when the CHANGELOG says so.
