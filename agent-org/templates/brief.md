# Brief: <one-line deliverable>

The only thing a spawned agent knows is this brief plus the files it names. Write it for a reader
with no context.

- **Member:** <id> (charter: `agent-org/members/<id>.md`)
- **Model:** <cheap for Workers; mid-tier only with a reason: ...>
- **Started by:** <member>; report back to it, never to the owner.
- **Branch / worktree:** <name>, based on <base>
- **Time budget:** <30> minutes, hard stop at <time>.

## Goal

<What done looks like, in one or two sentences.>

## Already decided (do not reopen)

- <register row ids and one-line summaries>

## Inputs

- <absolute or repo-relative paths, links, commands>

## Context boundary

This brief and the named inputs are the complete working context. Do not infer unstated product
intent or reopen the listed decisions. If the brief is ambiguous, report the ambiguity to the
member in **Started by** with a recommendation. Never ask the owner directly.

## Deliverable and acceptance check

- <the one deliverable>
- <how the spawner will verify it: a command, a test count, a file that must exist>

## Rules

- Follow `agent-org/OPERATING.md`. Commit after every green step.
- Do not dispatch another member. Only the top-level session starts agents.
- Decide anything inside your charter yourself and record it in the register. Route anything you
  cannot decide to the member that started you, with your recommendation. Never ask the owner.
- Anything ambiguous that blocks the deliverable: stop, report blocked with what you need.
- At the time budget: commit what is green, update your section of `agent-org/state-of-play.md`,
  report.

## Report format

- **Done:** what is committed (commit ids), acceptance check result with numbers.
- **Decided:** register rows added.
- **Open:** what remains, and what you need from whom.
