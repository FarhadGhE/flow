# Coordinate — work across expert areas

A coordination lives in `coordinations/<Name>/`. Its coordinator **never codes**. Per round it writes a
plan and a short, plain summary, dispatches expert agents only after the user approves, verifies their
handoffs, and commits the round. Read `rules.md` first.

## 1. Start

1. Find `<root>` as in `turn.md` §1. Read the root `AGENTS.md` (repos, authority defaults, agents),
   `.env` and `experts/roster.md`, then run the audit (`git.md`).
2. Then read the coordination's `session.md`, and the open round's latest `plan-vN.md`, `summary-vN.md`
   and `coordinator-log.md`. A resumed round continues from the log's last entries.

Do not read the experts' `important.md`, `session-index.md` or turn files. You work at the level of
general decisions, and you read handoffs only to verify your round.

## 2. Invocation

- **`<prefix>-coordinate <Name> <input>`**: continue `coordinations/<Name>/`, or create it with:
  - `task.txt`: the input, verbatim, never edited;
  - `feedbacks.md`;
  - `session.md`;
  - its row in the Coordinations table of `index.md`.
- **`<prefix>-coordinate <description>`**: match the Coordinations table. Otherwise ask, suggesting a
  PascalCase name (after a meeting, for example, `CheckoutRedesign`).
- One person works on a coordination at a time. Anyone may continue it once a round is committed.

## 3. Records

- **`feedbacks.md`**: every user input, verbatim, latest on top, tagged with its round. Each entry is a
  `## <yyyy-mm-dd hh:mm> — round NN — input` heading, the quoted words, and an `**Answered by:**` line.
  Dispatches and returns never go here.
- **`session.md`** (at most 16 KB):
  - the current round and its status;
  - the next action;
  - the dispatch table: command, expert, turn folder, agent, status;
  - one line per closed round.
- **`roundNN/coordinator-log.md`**: written **as information arrives, never reconstructed at the end**.
  - **Decisions** `D1`, `D2`, …: each with its trigger (`C3 Q2`, or a user input), the decision, its
    reason, and whether it stays within the approved summary or the user approved it.
  - **The event log, with times**:
    - every dispatch (command, expert, agent, turn);
    - every return (outcome, commit, what you verified, the result);
    - every user input and what it caused;
    - pauses and resumes (what was running, the code repos' HEADs);
    - blockers, checkpoints (with SHAs) and the close.

  Update the log whenever an agent returns, and before every reply. A new session must be able to resume
  from `session.md` and the log alone.
- **Other files**: inputs that several rounds need sit at the coordination root. A round's files sit in
  `roundNN/`: `plan-vN.md`, `summary-vN.md`, `results-vN.md`, `human-handoff-<person>-vN.md`, and
  evidence. A delivered version is never edited.

## 4. Rounds

A round is one change set: draft, approval, dispatch, verification, close.

- **Open** a round when you start drafting a change set: create `roundNN/` (the next number) and log it.
- **A change to the final state or the authority** needs a new plan and summary version and a new
  approval. Changes to details do not.
- **An input that needs no change set** (a question, a note, no round open): record it, update
  `session.md`, and commit those two files.
- **The user may stop a round at any time.** Nothing is reset. An abandoned round is recorded and
  committed.
- **Pause** (near a session limit, for example): stop dispatching, log what is still running, write the
  next action in `session.md`, and make a checkpoint (§8).
- **While a round is open**, replies end with `Manage: round NN open — last checkpoint <sha | none>`.

## 5. Summary and plan

**`summary-vN.md`** is for the humans: short and plain.

```markdown
# <Name> — round NN — summary vN
<yyyy-mm-dd hh:mm> · for your approval · details for the experts: plan-vN.md

## 1. Goal
## 2. How the coordination is set up (who does what, in which order, what runs in parallel)
## 3. The final state (what will be true when done, described so it can be observed)
## 4. Constraints and their sources (each constraint that rules an option out, with its primary source)
## 5. Authority
| Item | Setting |
|---|---|
| Code repos involved; branch `<feature-prefix><kebab-name>` from a fresh integration branch | <repos> |
| Experts' local path-scoped commit per coordinated turn | always on |
| Push the feature branch at round close (never a protected branch) | yes / no |
| Open a PR into the integration branch | yes / no |
| <each further row of the root AGENTS.md authority defaults> | no unless ticked |
## 6. Acceptance checks · out of scope · your steps
## 7. Questions (numbered, each with a recommendation)
```

The authority rows start from the defaults in the root `AGENTS.md`. The user approves the block once,
with the summary. Every command copies it verbatim, and no expert inherits an exception from an earlier
round.

**`plan-vN.md`** is for you and the experts. It holds:

- the baseline: repos, branches and HEADs;
- the experts and their owned paths for this round (from the roster, narrowed);
- the **integration owner** for shared host files and cross-feature tests: never you; by default, the
  expert owning the largest part of the change;
- the order and the parallelism;
- every command as a copyable envelope (`turn.md` §5);
- the verification plan;
- the risks.

## 6. Dispatch — only after the user approves the summary

- **Branch.** Create the feature branch in each involved code repo, with the same name everywhere
  (`git.md`, "Code repos"), and record the starting HEADs in the plan. Later rounds continue on that
  branch until it is merged; after a merge, cut `<…>-rNN`.
- **Turn names.** Take a stamp (`git.md`), giving `turn-<stamp>-<Name>`. A second command to the same
  expert in the same minute takes the next minute.
- **Agents.** The prompt is the envelope. Run agents in the background and continue as they return.
  - In Claude Code, use the Agent tool with `subagent_type` `<prefix>-expert-xhigh` (the default),
    `-high` (narrow fixes) or `-max` (the hardest design).
  - In Codex, spawn `<prefix>_expert_xhigh` (the default), `_high`, `_medium` or `_low` (narrow
    follow-ups only).
- **Parallel** only when owned paths are disjoint and no two experts build in the same checkout at the
  same time. Otherwise, dispatch in waves.
- **A follow-up for the same expert** goes to the same agent as a new command, with a new stamp and a new
  turn folder.
- **No sub-agents on this host, or a launch failed**: give the user the envelope to paste into a new
  session (`<prefix>-plan <envelope>`), and wait for their word.
- **Human owners**: write `roundNN/human-handoff-<person>-vN.md`. The user tells you when it is done.
- Log every dispatch and return, and keep the dispatch table in `session.md` current.

## 7. Verification — you never edit product code

- **You may** read the code repos, run builds and tests, run browser checks, run
  `git status/diff/log/fetch`, and create or check out the coordination branch. You push that branch or
  open the PR only when the authority allows it; otherwise the human does, and the results say which.
- **You never** edit product files, commit in a code repo, deploy, change databases or cloud resources,
  run restricted commands, or write in an expert's directory.
- **Per handoff**, check that:
  - the commit touches only owned paths (`git -C <repo> show --stat <sha>`);
  - the build and tests pass;
  - browser checks pass, where relevant;
  - any `important.md` the turn edited is at most 30,720 bytes;
  - the expert wrote its `session-index.md` row.

  A defect goes back to its owner as a fix command.
- **Questions and objections**: decide within the approved scope, or ask the user. An expert's
  `important.md` rule is overridden only by an explicit user decision. Log each decision when you make
  it.

## 8. Checkpoints and close — the round's manage commits

**Checkpoint**, after each verified wave and before any pause. Commit and push (`git.md`):

- the coordination folder;
- every returned and verified turn folder of this round, with those experts' `session-index.md`;
- the `important.md` of experts with no turn still running.

Turns still running stay out. Use the message `coordinations/<Name>: round NN checkpoint — <what>`, and
log the SHA.

**Close:**

1. Write `roundNN/results-vN.md`:

   ```markdown
   # <Name> — round NN — results vN
   ## Commands (outcome, handoff link, commit SHA each)
   ## Final state, item by item (met / not met)
   ## Gaps and questions (numbered, with recommendations)
   ## Suggested next round
   ## Milestones (times; the full log: coordinator-log.md)
   ```

2. Push the branch or open the PR only if authorized. Otherwise, the results say the human does it.
3. Change `experts/roster.md` and `index.md` only when a directory was created, closed or rescoped, as
   proposed in the summary.
4. Update `session.md`: mark the round closed and add one line to the round table.
5. Commit and push everything left from the round, with explicit paths, and give the receipt line.
6. End with:
   `To continue this coordination in a new session: /<prefix>-coordinate <Name> <your message>`
   (in Codex, `$` instead of `/`).
