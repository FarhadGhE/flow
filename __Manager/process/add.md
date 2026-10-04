# Add — bring another directory into the project

A direct turn in `__Manager` (`turn.md`) that runs this procedure. Nothing is created before the user
approves.

1. **Ask** for whatever is missing: the absolute path, the kind (code, documents, other), whether it
   exists or is planned, and its Git URL.
2. **Discover** it (`setup.md` step 3) and write `discovery-<name>-v1.md` in this turn folder.
3. **Propose** only the changes, in `add-proposal-v1.md`, using the setup proposal's shape: new experts,
   wider owned paths for existing experts, new shared host files, the settings and authority rows for a
   new code repo, and its new `.env` key. A change request produces the next version.
4. **After approval**, create or update:
   - the experts (`experts/<Name>/`) and `experts/roster.md`;
   - `index.md`;
   - the root `AGENTS.md` settings;
   - `.env.example` and `README.md`;
   - the `## Scope` of each existing expert whose owned paths grew (the approval covers this edit).

   Then add the new key to this machine's `.env`.
5. **Verify** (`setup.md` step 9), commit and push. On other machines, `join.md` asks for the new key
   the next time a session starts there.
