---
name: to-progress
description: "Turn an approved set of tickets into a progress.md ledger that an autonomous orchestrating agent can run: frontier, claims, human gates, project rules, activity log."
disable-model-invocation: true
---

# To Progress

Write one **ledger**, `progress.md`, for a set of tickets, so the user can later hand the whole job to an autonomous **orchestrator** with one sentence: "work through `<path>/progress.md`". The orchestrator works the **frontier** (tickets whose blockers are all `done`), dispatches or does each ticket, and keeps the ledger current. Hand-offs between agents and sessions go through the ledger, so it carries everything a cold agent needs to pick the next ticket safely, and nothing it can look up cheaply.

**Single source of truth:** ticket files own scope, acceptance criteria and blocking edges. The ledger owns progress, claims, gates and the log. The ledger links to tickets and mirrors only their titles and blockers; acceptance criteria stay in the tickets.

## Steps

### 1. Gather the tickets

Use the tickets named in the arguments or the conversation. The default location is `.scratch/<feature-slug>/issues/`, the output of `/to-tickets`; on a real tracker, use the issue identifiers/URLs. For each ticket, read its title, `Blocked by`, status label (`ready-for-agent`, `ready-for-human`, or similar) and the parent spec it references.

Done when every ticket has its ID, title, link, blockers and mode (agent or human), and the blocking edges form no cycle.

### 2. Survey the project

The orchestrator starts cold. Collect the facts it can't find with one command, plus the guardrails a mistake would break:

- **Workspace:** repository root; where the deliverable code lives versus history or reference material; how to start the local environment and run the checks the tickets name.
- **Version control:** VCS state, branch, commit convention (e.g. `NN: …` per ticket), staging rules imposed by hooks, files that must never be tracked. If there's no VCS, record how results are referenced instead.
- **Side effects:** systems a run can touch for real (production servers, CRMs, payment providers, email recipients, DNS, live hosting) and what each run may do there.
- **Safety:** active command hooks or permission rules (read the user's settings and recent hook denials) and the safe alternative for each.
- **Sensitive data:** paths holding credentials, personal data or production dumps, and where each may go.
- **Tooling:** the language, runtimes and test style the tickets must follow; user preferences from `CLAUDE.md`/`AGENTS.md` that bear on the work.
- **Documents:** the spec, research and test-plan files, and the editing rule for each (e.g. "current state only, rewrite in place").

Leave out what the environment already states (`package.json` scripts, `--help`, the directory listing): the ledger caches only the unwritten convention, the reason and the gotcha.

Done when every guardrail you found maps to a rule or a gate, and every rule states a fact the orchestrator couldn't get from one lookup.

### 3. Define the human gates

A **gate** is a human decision or input a ticket needs before part of it can run: access, credentials, go/no-go, coordination with third parties, any irreversible or outward-facing action. Give each gate an ID (e.g. `H07` after the ticket that needs it, or `H-<TOPIC>` for a standing rule that spans tickets), a state (`pending`, `granted`, `standing rule`) and the exact scope or input needed. A grant covers only its stated scope.

Every `ready-for-human` ticket gets a gate. Agent tickets get one where a step crosses a side effect from step 2.

Done when every human-only action in every ticket sits behind a gate, and every gate in the table is referenced by at least one ticket row or standing rule.

### 4. Write the ledger

Write `progress.md` next to the tickets folder (`.scratch/<feature-slug>/progress.md`), following the template in [`TEMPLATE.md`](TEMPLATE.md). Fill it with the facts from steps 1–3:

- **Current checkpoint:** done count, the next frontier ticket(s), active claims, and any work that already exists (commits, partial fixes).
- **Protocol:** the template's orchestrator loop. Adapt the commit and verification wording to the project.
- **Rules:** the facts from step 2, grouped by topic, each phrased as the behaviour to follow.
- **Task table:** one row per ticket in dependency order. Blockers are mirrored exactly; mode, progress and gates come from steps 1 and 3. Record existing partial work in the claim/commit column.
- **Gates**, **checkpoints** (milestones the tickets imply: cutover, release, review dates; omit the section if there are none) and an **activity log** whose first row records the ledger's creation.

### 5. Check and record

Check that:
- the table holds every ticket exactly once;
- each row's blockers match its ticket file;
- every link resolves;
- every gate ID in a row exists in the gate table;
- the checkpoint's "next" is a frontier ticket.

Commit the ledger if the project uses VCS, staging its path explicitly. If you corrected a ticket file (a broken blocker, a missing status), commit that with it.

Report to the user:
- the ledger path;
- the frontier;
- gates that block the frontier right now, with the input each needs;
- the one-sentence prompt to start the orchestrator.
