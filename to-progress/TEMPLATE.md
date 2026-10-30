# progress.md template

Replace `<…>` placeholders. Keep the protocol's structure; tune its wording to the project (commit convention, verification commands). Drop rule topics that don't apply, and add the project's own.

```markdown
# <Feature name>: progress

**Parent:** [<Spec title>](<relative link to spec>). Background: <research/decision docs, with links>. Manual test scenario: <link, if any>.

**Scope:** <N> tickets in `issues/` (or <tracker + query>), numbered in dependency order. `ready-for-human` tickets are performed by the owner; an agent prepares inputs and checklists for them.

## Current checkpoint

- **Done: 0/<N>. Next: <frontier IDs>. Active claims: none.**
- <Existing work: commits, partial fixes, or "none".>

## Orchestrator protocol

When asked to work through this ledger (or to "implement the next step"):

1. Read this file. Pick the frontier: tickets that aren't `done` and whose blockers are all `done`, lowest ID first. Read the chosen ticket, then the parts of the spec it references.
2. If the ticket is `human`, or a step needs a gate that isn't `granted`: prepare everything that can be prepared locally, set `waiting_human`, write one precise next action in the log, and move to another frontier ticket or stop.
3. Claim it: set `in_progress` and your worker ID in the table. Parallel workers each take a different frontier ticket; only the orchestrator writes this ledger, and workers report results back to it.
4. Implement the slice. Run the checks the ticket names.
5. Tick acceptance boxes in the ticket file only for verified work or an explicit owner waiver.
6. Commit (see Rules → Version control), set `done`, put the commit reference in the table, update "Current checkpoint" and add one log line.
7. On a blocker: log the blocker with one next action, set `waiting_human` or `waiting_external`, release the claim, and continue with another frontier ticket or stop.

Stop when the frontier is empty, or when every remaining frontier ticket waits on a human. Report the gates that need input.

## Rules

**Workspace**
- <Root, where deliverable code lives, how to start the environment, how to run the checks.>

**Version control**
- <VCS, branch, commit convention, explicit staging, never-tracked paths, how to revert.>

**Side effects and sensitive data**
- <Each external system and what a run may do there. Data paths and where they may go.>

**Safety**
- <Hook-blocked commands and the safe alternative for each.>

**Tooling**
- <Language, runtime, test style, user preferences that apply.>

**Documents**
- <Editing rule per document.> Ticket files own scope and acceptance criteria; this ledger owns progress, claims, gates and the log.

**Progress values:** `todo`, `in_progress`, `waiting_human`, `waiting_external`, `done`.

## Task table

| ID | Ticket | Blocked by | Mode | Progress | Gate | Claim / commit |
|---|---|---|---|---|---|---|
| 01 | [<title>](issues/01-<slug>.md) | — | agent | todo | — | — |

## Human gates

A grant covers only its stated scope.

| Gate | State | Scope / input needed |
|---|---|---|
| H<NN> | pending | <exact access, decision or input> |

Open questions affecting tickets: <question → which ticket it affects and the default until it's answered>.

## Checkpoints

- <Milestone>: not reached. <Rollback target, review date rule, etc.>

## Activity log

One line per hand-off: result, blocker or next action.

| When | Ticket | Result / next action |
|---|---|---|
| <YYYY-MM-DD> | — | Ledger created. Next: <frontier>. |
```
