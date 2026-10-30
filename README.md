# skills

Personal agent skills. This repository is the source of truth; local installs come from here via [skills.sh](https://skills.sh).

## Skills

| Skill | Purpose |
|---|---|
| [`to-progress`](to-progress/SKILL.md) | Turn an approved set of tickets into a `progress.md` ledger that one orchestrating session can run end to end. |
| [`linkedin`](linkedin/SKILL.md) | Write and gatekeep LinkedIn posts in a configured voice. |

## `to-progress`: the step after `to-spec` → `to-tickets`

`to-progress` extends Matt Pocock's [skills](https://github.com/mattpocock/skills) pipeline:

```
/to-spec  →  /to-tickets  →  /to-progress  →  "work through .scratch/<feature>/progress.md"
 spec         tickets          ledger            one orchestrating session
```

1. `/to-spec` captures the plan as a spec.
2. `/to-tickets` breaks it into tracer-bullet tickets with blocking edges.
3. `/to-progress` writes `progress.md` next to the tickets: the **frontier** (tickets whose blockers are all `done`), claims, **human gates**, project rules and an activity log.

After that, a single session can orchestrate the whole job. It reads the ledger, works the frontier, dispatches or does each ticket, commits, and updates the ledger. It stops at human gates and reports the input each one needs. Because hand-offs go through the ledger, a cold agent or a new session resumes without the original conversation.

Division of ownership: ticket files own scope, acceptance criteria and blocking edges. The ledger owns progress, claims, gates and the log.

Requires `to-spec` and `to-tickets` from Matt Pocock's skills; `to-progress` reads the ticket files `to-tickets` produces (`.scratch/<feature-slug>/issues/`) or tickets on a real tracker.

## Install

```sh
pnpm dlx skills add <path-or-repo> -g -s to-progress
```

Local, from a checkout (this is how the installed copy is kept in sync with the repo):

```sh
pnpm dlx skills add ~/_work/skills -g -s to-progress
```

After editing a skill here, re-run the command to refresh the installed copy.
