# Turn — work in one area

- A **direct turn** is one conversation in one directory (`experts/<Name>` or `__Manager`). Every message
  of the conversation continues the same turn folder; a new conversation starts a new one. An expert's
  direct turn may change code inside that expert's owned paths, after the user's go (§3).
- A **coordinated turn** is one `FLOW COMMAND` from a coordinator in dispatch mode (§5).

Read `rules.md` first.

## 1. Start

1. **Find `<root>`.** Use the path given by the per-machine command; otherwise run
   `git rev-parse --show-toplevel` in the current folder. Use absolute paths everywhere, and one
   self-contained shell command per call.
2. **Read** the root `AGENTS.md` (project settings and rules) and `.env`. If `.env` is missing, run
   `join.md` first.
3. **Run the audit** (`git.md`).

## 2. What is asked

- **The message starts with `FLOW COMMAND`**: a coordinated turn (§5). Its `Project:` must be this
  repo's prefix.
- **The first word is `<Dir>/turns/<turn-folder>`**: continue that direct turn (§3).
- **The first word names a directory in the root `index.md`** (any case; `Billing` means
  `experts/Billing`): start a new direct turn there.
- **Otherwise**, read the root `index.md` and pick the best row.
  - If you are confident, say which directory and why, in one line, then start.
  - If you are unsure, or two rows fit, ask the user. Offer the candidates, plus a new expert with a
    suggested PascalCase name. Create only what they choose (§6).
- **The request needs two or more experts**: suggest `<prefix>-coordinate <Name> <request>` and stop,
  unless the user wants it handled in one directory (reading other areas, changing code only in its
  own). `coordinations/` belongs to `coordinate.md`.

## 3. Direct turn

**Read as little as possible:**

1. `important.md` first;
2. only the top `session-index.md` rows you need;
3. only the turn files you choose, typically the `session-summary.md` of the turn you continue.

Never read a whole `turns/` folder.

**Folder.** A new turn takes a stamp (`git.md`) and creates
`<Dir>/turns/turn-<stamp>-direct-<FLOW_DEVELOPER>/`. A continued turn keeps its folder.

**Records, all in the turn folder:**

- **`feedbacks.md`**: every user input of this turn, verbatim (typos kept, never summarized), latest on
  top. Write each entry when the input arrives:

  ```markdown
  # feedbacks — <Dir> — turn <stamp> <source>

  ## <yyyy-mm-dd hh:mm> — input
  > the user's words, verbatim, however long

  **Answered by:** `<file>`, `<file>`
  ```

- **`session-summary.md`**: the turn's state, updated after every input, at most 16 KB:

  ```markdown
  # Turn <stamp> — <source> — <Dir>
  About: <one line; it becomes the session-index row>
  Status: IN PROGRESS | DONE | PARTIAL | BLOCKED
  Continues: <an earlier turn folder, or —>

  ## What was asked and decided
  ## Done (deliverables in this folder, commit SHAs)
  ## Open items
  ## Resume notes
  ```

- **Deliverables**: new versioned files (`investigation-result-v1.md`, `<topic>-plan-v1.md`, …), never
  edited once delivered. A canonical document is re-issued as its next version, and `important.md` names
  the current one.
- **`handoff.md`**: only when another directory needs to know a decision (template in §5).
- **`important.md`**: only when genuinely needed (§7).

**Code changes.** A direct turn changes code only in an expert directory, and only inside that
expert's owned paths (`experts/roster.md`):

1. Write `change-plan-vN.md` in the turn folder: the goal, the paths to change, the branch, the checks,
   and an authority table that starts from the root `AGENTS.md` defaults. Show it, and wait for the
   user's go. A change request produces the next version.
2. After the go, create the feature branch (`git.md`, "Code repos"), make the change, and run the build
   and tests, plus a browser check where the change shows.
3. Commit once per code repo, path-scoped (`git.md`, "Code repos"), and record the SHAs in
   `session-summary.md`.
4. Push or open a PR only if the plan's authority says so.

A change that needs another expert's paths is a coordination: suggest
`<prefix>-coordinate <Name> <request>`.

**Close every round** (each user message):

1. Give the `feedbacks.md` entry its `Answered by:` line, and bring `session-summary.md` up to date.
2. Make sure this turn's row is at the top of `<Dir>/session-index.md` and current:
   `- [<turn>](turns/<turn>/session-summary.md) — <Status> — <About>`.
3. Commit and push only your paths (`git.md`): the turn folder, `<Dir>/session-index.md`, and
   `<Dir>/important.md` if you edited it.
4. End the reply with the receipt line, then:
   `To continue this turn in a new session: /<prefix>-plan <Dir>/turns/<turn-folder> <your message>`
   (in Codex, `$` instead of `/`).

## 4. Implementation plans — only when the user explicitly asks

- **The plan**: write `<topic>-implementation-plan-vN.md`. It must be decision-free:
  - every file's full content or exact change;
  - the allowed directories;
  - the Git limits: branch create and checkout only, no code commits or pushes;
  - paths given as `.env` keys;
  - one self-contained command per step;
  - a base-commit guard;
  - a report template the executor fills as `<topic>-implementation-report.md` in the same folder.
- **The kickoff**: also write `agent-kickoff-prompt-vN.md`, one copyable block for an executor that the
  human starts. It tells the executor to:
  - apply the plan literally, deciding nothing and asking nothing;
  - touch only the named directories;
  - never commit or push code repos;
  - report success or any issue in the report file.

  Earlier plans' uncommitted changes may already be in the working tree; the guard says what must hold.
- **Never** execute the plan yourself or start the executor. To review a run, verify independently by
  comparing the plan's file blocks with the working tree.

## 5. Coordinated turn (`FLOW COMMAND`)

The envelope:

```text
FLOW COMMAND
Project: <prefix> · Coordination: coordinations/<coordination folder> · Round: <NN> · Command: C<n>
Expert: <Expert> · Agent: <agent name>
Turn: turn-<stamp>-<Name>
Read: <only the files you must read>
Owned paths: <repo key>: <globs>; …
Do not touch: <paths>
Authority: <copied verbatim from the approved summary>
Task: <paragraphs>
Done when: <checks>
Handoff for: <consumers>, coordinator
```

1. **Read** the root `AGENTS.md`, `.env` and `experts/<Expert>/important.md`, then only the files under
   `Read:`; anything else only when genuinely needed. If `experts/<Expert>/` does not exist, create it
   (§6) with the envelope as its `task.txt`.
2. **Create** `experts/<Expert>/turns/<Turn>/`. Write `session-summary.md` with
   `Status: IN PROGRESS`, and `command.md`:

   ```markdown
   # Command C<n> — <Name> round <NN> → <Expert>
   <yyyy-mm-dd hh:mm> · agent <agent name>

   ## Input (verbatim)
   <the whole envelope>

   ## Decision summary
   - approach; key decisions and why; boundaries kept; risks
   ```

3. **Work** only inside the owned paths and the authority. In a shared checkout, retry on lock errors
   (`git.md`), and never delete another expert's output.
4. **Commit** once in each code repo you changed, with only the owned paths changed in this turn
   (`git.md`). Make no commit when nothing changed. Never push, merge, open a PR, reset, stash, clean or
   switch branches.
5. **Write `handoff.md`.** It must be self-contained: its consumers read nothing else of yours.

   ```markdown
   # Handoff C<n> — <Expert> → <consumers>, coordinator
   <yyyy-mm-dd hh:mm> · <Name> round <NN> · Outcome: DONE | PARTIAL | BLOCKED
   Code: `<branch>` @ `<sha>` (<repo key>) — or "no code change"

   ## Decisions others need
   ## For consumers (interfaces, routes, configuration — links to code, minimal examples)
   ## Verified (commands and real results; skips with reasons)
   ## Questions (numbered, each with a recommendation)
   ## Objections to the coordinator's decisions
   ## Next
   ```

6. **Finish** `session-summary.md` and write your row in `experts/<Expert>/session-index.md`. Touch
   `important.md` only if a later session genuinely needs it (at most 30,720 bytes).
7. **Return** at most 10 lines: the outcome, the handoff path, the commit SHA(s), and the number of
   questions and objections.

Further rules for a coordinated turn:

- Make no manage commit; the coordinator commits the round. Add no resume line.
- You cannot ask the user. Questions and objections go in the handoff.
- A command that conflicts with the rules, the root `AGENTS.md` or your `important.md` gets an Objection
  and a PARTIAL or BLOCKED outcome, never silent obedience.
- Start no sub-agents unless the command allows it.
- To read another directory's handoff, use the one the command names; otherwise the last
  `<Dir>/turns/turn-*/handoff.md` by name.

## 6. A new expert

Create `experts/<Name>/` (PascalCase) with:

- `task.txt`: the first message or command, verbatim, never edited;
- `important.md`, from `templates/important.md`;
- `session-index.md`, from `templates/session-index.md`;
- then its first turn.

Add its rows to the root `index.md` and `experts/roster.md` once the user has approved its owned paths.
In a coordination, the coordinator adds them at the round's close. Commit them with the turn.

## 7. `important.md`

Add only what a later session here must know quickly: a rule, a reason or a questionable decision.
Keep it generic and current, never a history, and replace the related entry when a decision changes.
The hard cap is 30,720 bytes; make room by merging or removing rules, never by splitting the file. The
root `AGENTS.md` and `rules.md` take precedence over it, and an explicit user decision replaces an entry.

## 8. Close-out

When the user says the work is done:

- set `Status: DONE` and carry the open items forward;
- put durable rules into `important.md`. A rule the user locked for the whole project goes into the root
  `AGENTS.md` "Project rules", with their OK;
- change the root `index.md` row only if the scope changed;
- record the message and close the round.
