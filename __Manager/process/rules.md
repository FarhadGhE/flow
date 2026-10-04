# Rules — for every session in a manage repo

These rules are short on purpose; the procedures are in this folder. Precedence: the user's explicit
instruction (for the scope it names), then the project rules in the root `AGENTS.md`, then this file,
then an expert's `important.md`.

## 1. What a manage repo is

- The project's shared memory in Git, always committed and pushed. Nothing lives only in chat or in an
  agent's memory: inputs are recorded verbatim, results are files, and every round is committed.
- The code repos listed in the root `AGENTS.md` change only in coordinated turns, on a feature branch,
  by the expert who owns the paths.

## 2. Machine values: `.env`

- `.env` is never committed. It holds this machine's values: `FLOW_DEVELOPER` (your handle), one
  absolute path per involved directory (`REPO_<KEY>`, `DIR_<KEY>`), and toolchains that are not on PATH
  (`TOOL_<NAME>`). `.env.example` lists the keys.
- Read `.env` at the start of every session. If the file or a key is missing, run `join.md` or ask.
  Never guess.
- Never put an absolute machine path, a secret or a `.env` value in a committed file. Name the key
  instead.

## 3. Routing and reading

- Find the right directory through `index.md` only. Never scan the tree.
- In a directory, read `important.md` first, then only the `session-index.md` rows and turn files you
  need. Never read a whole `turns/` folder. Open `archive/` only for a specific need.

## 4. Shapes

- An expert is `experts/<Name>/` (PascalCase). It holds `task.txt` (its first message or command,
  verbatim, never edited), `important.md`, `session-index.md`, `turns/` and `archive/`, and nothing else
  at its root. `__Manager/` keeps the same records, plus `AGENTS.md` and `process/`.
- A turn folder is `turns/turn-yyyy-mm-dd_hh-mm-<source>/`, stamped in the project's time zone
  (`git.md`). `<source>` is `direct-<FLOW_DEVELOPER>` for a direct turn (one conversation) or the
  coordination's name for a coordinated turn (one command).
- A coordination is `coordinations/<Name>/`. It holds `task.txt`, `feedbacks.md`, `session.md`, shared
  inputs and `roundNN/` folders.
- Deliverables are versioned (`<topic>-vN.md`) and live in their turn or round folder. A delivered
  version is never edited; a revision is the next version and stands alone.
- `index.md` (routing, no status) and `experts/roster.md` (owned paths) change only when a directory is
  created, closed or rescoped.

## 5. Process

1. When the user asks for a plan, the plan is the deliverable. Don't execute it and don't start an
   executor.
2. Code repos: no commits, except a coordinated turn's one local, path-scoped commit on its feature
   branch. No pushes unless the round's authority allows it, and then only the feature branch, never a
   protected branch.
3. The project's restricted commands (listed in the root `AGENTS.md`) run only when a round's
   authority grants them.
4. Do only what the task names. An unnamed step, however natural, is not yours.
5. The manage repo is always committed and pushed. A direct turn commits at the end of every round (each
   user message); a coordination commits at its checkpoints and at its close. Commit only your own paths
   (`git.md`).
6. Respect any directory limits a task sets ("work only in …").
7. Outward actions need the user's yes: creating a remote or pushing to a new one, contacting people,
   paid calls beyond an approved budget, installing anything outside this repo.
8. A constraint that rules out an option (a vendor, model, technology, region, extra cost or cut scope)
   needs a primary source, quoted with a link: a contract clause, a client message or a user decision.
   A note that only repeats the constraint is not a source. An unsourced constraint becomes a numbered
   question.

## 6. Coordinations (details in `coordinate.md`)

- Work that needs two or more experts is a coordination; work in one area is a turn. `__Manager` is
  never coordinated.
- The coordinator never codes: no product edits, code commits, deploys, database or cloud changes,
  restricted commands, or writing in an expert's directory. It only commits the round's files.
- Each round has a plan and a short, plain summary with an authority block. Dispatch happens only after
  the user approves. Every command copies the authority verbatim, and no expert inherits an exception
  from an earlier round.
- Experts decide the details and write the code inside their owned paths. They cannot ask the user:
  questions and objections go in the handoff. A command that conflicts with the rules gets an Objection,
  never silent obedience.
- Every plan names an integration owner for shared host files. It is never the coordinator.
- One person works on a coordination at a time (a team agreement).

## 7. Start and end of a session (these replace scripts)

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
- Only sessions working in that directory edit it. Others don't read it by default.

## 9. `session-index.md`

- One row per turn, newest first: `- [<turn>](turns/<turn>/session-summary.md) — <Status> — <About>`.
- Each turn writes only its own row and refreshes it at every commit. Git's union merge keeps rows from
  other machines. If a turn is listed twice, keep the row that matches its summary.

## 10. Sizes and the harness

- `session-summary.md` and a coordination's `session.md` stay at or below 16 KB; condense them when they
  grow past that.
- The shell's working directory may not persist between calls. Use one self-contained `cd <abs> && …`
  per command, and absolute paths in every file write.
- Sub-agents cannot ask the user anything.
- `style.md` defines how deliverables and replies look.
