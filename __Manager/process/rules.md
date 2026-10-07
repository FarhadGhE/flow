# Rules — for every session in a manage repo

These rules are short on purpose; the procedures are in this folder. Precedence: the user's explicit
instruction (for the scope it names), then the project rules in the root `AGENTS.md`, then this file,
then an expert's `important.md`.

## 1. What a manage repo is

- The project's shared memory in Git, always committed and pushed. Nothing lives only in chat or in an
  agent's memory: inputs are recorded verbatim, results are files, and every round is committed.
- The code repos listed in the root `AGENTS.md` change only on a feature branch, after the user's go on
  a written plan, and only inside the owned paths of the experts that plan names (`git.md`, "Code
  repos").

## 2. Machine values: `.env`

- `.env` is never committed. It holds this machine's values: `FLOW_DEVELOPER` (your handle), one
  absolute path per involved directory (`REPO_<KEY>`, `DIR_<KEY>`), and toolchains that are not on PATH
  (`TOOL_<NAME>`). `.env.example` lists the keys.
- Read `.env` at the start of every session. If the file or a key is missing, run `join.md` or ask.
  Never guess.
- Never put an absolute machine path, a secret or a `.env` value in a committed file. Name the key
  instead.

## 3. Routing and reading

- Find the right expert through the root `index.md`, and the right coordination through
  `coordinations/index.md`. Never scan the tree.
- In a directory, read `important.md` first, then only the `session-index.md` rows and turn files you
  need. Never read a whole `turns/` folder. Open `archive/` only for a specific need.

## 4. Shapes

- `<stamp>` is a creation time in the project's time zone, written `yyyy-MM-dd_HH-mm` (`git.md`).
- An expert is `experts/<Name>/` (PascalCase). It holds `task.txt` (its first message or command,
  verbatim, never edited), `important.md`, `session-index.md`, `turns/` and `archive/`, and nothing else
  at its root. `__Manager/` keeps the same records, plus `AGENTS.md` and `process/`.
- A turn folder is `turns/turn-<stamp>-<source>/`. `<source>` is `direct-<FLOW_DEVELOPER>` for a direct
  turn (one conversation), or the coordination's `<Name>` for its work in that expert (one command, or
  one simple-mode round).
- A coordination is `coordinations/<stamp>-<Name>/`, with `<Name>` in PascalCase. It holds `task.txt`,
  `feedbacks.md`, `session.md`, shared inputs and `roundNN/` folders. `coordinations/index.md` lists
  every coordination.
- Deliverables are versioned (`<topic>-vN.md`) and live in their turn or round folder. A delivered
  version is never edited; a revision is the next version and stands alone.
- The root `index.md` (routing, no status) and `experts/roster.md` (owned paths) change only when a
  directory is created, closed or rescoped.

## 5. Process

1. **The user's go.** Code changes start only after the user's explicit go on a written plan: an
   expert's `change-plan-vN.md` in a direct turn, or a coordination round's `summary-vN.md`. Its
   authority block is what the go allows. "go", "approve" and "yes" all count.
2. When the user asks for a plan, the plan is the deliverable: don't execute it, and don't start an
   executor. When the user asks for a change, the change plan comes first (item 1).
3. Code repos: only local, path-scoped commits on the feature branch. No push, merge or PR unless the
   approved authority allows it, and then only the feature branch, never a protected branch.
4. The project's restricted commands (listed in the root `AGENTS.md`), deploys, and database or cloud
   changes run only when the approved authority grants them.
5. Do only what the task names. An unnamed step, however natural, is not yours.
6. The manage repo is always committed and pushed. A direct turn commits at the end of every round (each
   user message); a coordination commits at its checkpoints and at its close. Commit only your own paths
   (`git.md`).
7. Respect any directory limits a task sets ("work only in …").
8. Outward actions need the user's yes: creating a remote or pushing to a new one, contacting people,
   paid calls beyond an approved budget, installing anything outside this repo.
9. A constraint that rules out an option (a vendor, model, technology, region, extra cost or cut scope)
   needs a primary source, quoted with a link: a contract clause, a client message or a user decision.
   A note that only repeats the constraint is not a source. An unsourced constraint becomes a numbered
   question.

## 6. Coordinations (details in `coordinate.md`)

- Work that needs two or more experts is a coordination; work in one area is a turn. `__Manager` is
  never coordinated.
- A coordination works in rounds. Each round runs in one mode, chosen from the task and stated in its
  summary. The user's request can set the mode, and the next round may use the other one.
  - **Simple mode**: the coordinator does the work itself, with the involved experts' context, and
    records it in each of those experts.
  - **Dispatch mode**: the coordinator dispatches the experts, verifies their handoffs, and never codes:
    no product edits, code commits, deploys, database or cloud changes, restricted commands, or writing
    in an expert's directory.
- Every round shows its summary, with an authority block, and acts only after the user's go. Every
  command copies the authority verbatim, and no round inherits an exception from an earlier one.
- In dispatch mode, experts decide the details and write the code inside their owned paths. They cannot
  ask the user: questions and objections go in the handoff. A command that conflicts with the rules gets
  an Objection, never silent obedience. Every dispatch plan names an integration owner for shared host
  files; it is never the coordinator.
- One person works on a coordination at a time (a team agreement).

## 7. Start and end of a session

- At the start of every session, run the audit in `git.md`. Report leftovers before any work. Finish
  your own first (with the user's OK); never touch other sessions'.
- At the end of every round, the receipt line (`git.md`) closes the reply. A reply without it is
  unfinished.

## 8. `important.md`

- It holds what a later session in this directory must know quickly: rules, reasons and questionable
  decisions. Keep it generic and current, never a history. A new decision is appended or replaces the
  related entry.
- Add only genuine context. Never write "nothing to add".
- Hard cap: 30,720 bytes. To make room, remove the similar rule, rewrite one rule to cover both cases,
  or remove the most useless or obsolete rule (Git keeps the old text). Never split the file.
- Only a session working in that directory edits it: a turn there, or a coordinator recording a
  simple-mode round. Others don't read it by default.

## 9. Row indexes: `session-index.md` and `coordinations/index.md`

- `session-index.md`: one row per turn, newest first:
  `- [<turn>](turns/<turn>/session-summary.md) — <Status> — <About>`.
- `coordinations/index.md`: one row per coordination, newest first:
  `- [<stamp>-<Name>](<stamp>-<Name>/session.md) — <round NN open | round NN closed> — <About>`.
- Each session writes only its own row and refreshes it at every commit. Git's union merge keeps rows
  from other machines. If a turn or a coordination is listed twice, keep the row that matches its
  `session-summary.md` or `session.md`.

## 10. Sizes and the harness

- `session-summary.md` and a coordination's `session.md` stay at or below 16 KB; condense them when they
  grow past that.
- The shell's working directory may not persist between calls. Use one self-contained `cd <abs> && …`
  per command, and absolute paths in every file write.
- Sub-agents cannot ask the user anything.
- `style.md` defines how deliverables and replies look.
