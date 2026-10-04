# Join — set up this machine for an existing manage repo

Use this when the repo is set up (`index.md` exists) but this machine has no `.env`, or `.env` lacks a
key that `.env.example` lists. Afterwards, continue with the user's original request.

1. **Handle.** Ask for the user's handle (letters and digits), suggesting one from
   `git config user.name`. It names their direct-turn folders.
2. **Directories.** For every key in `.env.example`, find this machine's copy. For a code repo, look one
   level deep in this repo's parent folder and in `~/src`, `~/code`, `~/projects`, `~/Projects`,
   `~/repos`, `~/dev` and `~/Documents/GitHub`. A folder matches when `git -C <dir> remote get-url origin`
   equals the remote listed in the root `AGENTS.md`. Ask for anything you cannot find, and offer to
   clone a missing repo (only with a yes).
3. **`.env`.** Write it from `.env.example`. Never commit it.
4. **Commands.** Offer the per-machine commands and expert agents (`setup.md` step 8), and install
   them after a yes. Tell the user to restart the agent.
5. **Reply** with what was set and the "Use it" table from `README.md`, then continue with the original
   request.

There is no manage commit, because no tracked file changed.
