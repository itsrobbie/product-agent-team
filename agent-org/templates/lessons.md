# Lessons

Corrections the owner has made, written as rules. They bind every member (`OPERATING.md`
section 11). Starting set: learned running a real product build with this team shape, with
project specifics removed. Add the repo's own corrections below the line.

## How the PO works with the owner

1. **Definition before plan before execution.** Bring the WHAT for approval, then the plan of who
   does what, then dispatch. Do not dispatch members to write plans before the definition is
   approved.
2. **Bring decision briefs, not maps.** Every fork comes with framing, what it blocks, real options
   with trade-offs, a recommendation with reasoning, the cost of deferring, and exact
   confirm/override wording. A one-line "A or B?" pushes the research back onto the owner.
3. **Drive completeness yourself.** Run gap audits across the whole surface, including areas
   nobody has raised yet (hosting, deploy, assets, branding), fill depth with members in parallel,
   and bring one batched pass. Do not resolve forks one at a time with "which way?" at the end of
   each turn.
4. **Decide members' issues yourself.** Resolve from the docs, the register, the owner's documented
   direction, or an educated call on the strategy, and record it. Do not relay members' questions
   to the owner, even when a member's report lists them as "needs owner".
5. **Strip mislabelled owner items.** An engineering "which pattern?" item (ADR, error contract,
   migrations, ID convention, test stack) listed as an owner question is the member's own homework.
   Send it back to the member to decide.
6. **Report short.** Solved, concluded, decided, needed from the owner. A few sentences.
7. **Do not re-ask what the owner declined.** Propose only changes the owner confirmed, and update
   the rules to match a declined change.
8. **When the owner rejects a workflow step as wrong, remove the step,** do not keep it as a gate.
   Fold it into an approval the owner already gives (usually a PR merge) and update every doc that
   told the owner to do it by hand.
9. **When the owner says to keep moving, do not block on tidiness** such as uncommitted work on a
   working branch.

## Git, CI and releases

10. **The team owns all git and GitHub work.** Branches, commits, pushes, PRs, CI, re-runs,
    releases, tags, cleanup. Open the PR as soon as the branch is pushed. The owner's only step is
    merging into `main`. Never ask the owner to open a PR or push anything.
11. **Releases are automated.** The version lives in a committed file and is bumped in a PR; merging
    to `main` with green CI creates the tag and release. No manual tag-pushing step.
12. **A merged PR is not proven unless CI finished green.** If `ci` is not a required check, a PR can
    merge with its run cancelled; re-run the latest `main` before relying on it.
13. **Reproduce CI's real conditions before pushing a CI change.** Same environment variables, same
    build path, same cache mode. A plain local build that passes says little about CI, and each
    failed CI cycle costs a full run.
14. **Cap every CI test step** with a timeout and a hang dump, so a stuck test fails fast and names
    itself instead of burning the run.
15. **Scrub identity before every push.** Generated files (bytecode caches, lock files) and docs can
    carry usernames, handles, absolute paths and hostnames.

## Running spawned agents

16. **One deliverable and a time budget per agent** (about 30 minutes, hard stop in the brief).
    Split long serial scopes (image builds, several test suites, many screenshots) into parallel or
    sequential smaller agents.
17. **Check elapsed time against the budget** whenever you check on agents. Flag and split anything
    more than about 5 minutes over. "Alive and committing" is not "on track".
18. **Do not trust an agent's "done" or "budget used up".** Check what it actually committed. If
    the deliverable is unfinished, start a fresh agent from the existing commits.
19. **Commit after every green step,** and give agents enough turns to finish. A low turn limit
    with no early commits strands large bodies of work.
20. **Never end a turn while your own agents are running.** A resumed or fresh agent picks up from
    the commits and reviews whatever earlier agents left unreviewed.
21. **If a session dies with work uncommitted,** confirm the worktree builds, checkpoint-commit it,
    then start a fresh agent from that checkpoint. Old sessions cannot be resumed.
22. **Cap concurrency to what the machine can hold** (about 4 workstreams on a laptop). Queued agents
    are expected, not a failure.
23. **For deep design work,** run the design leads at high reasoning effort from one shared brief
    listing what is already decided, have QA review the drafts mid-run for cross-cutting flaws, and
    have the PO integrate and resolve conflicts itself.
24. **Model tiers:** leads think on the top model; Workers do on a cheap model. Raise a model only
    when a task needs it, and say why.

## Testing

25. **Never hide the thing a test is meant to prove.** Making a flaky screenshot pass by hiding the
    subject, loosening tolerance or masking the region yields a green test that proves nothing. Make
    the render deterministic instead, and assert that the subject actually rendered.
26. **Derive timeouts from the knobs they depend on.** When a shared timing setting changes, every
    test waiting on that operation must compute its wait from the setting (count x delay + margin),
    not hard-code seconds.
27. **Flows that emit one-time secrets cannot be resumed from a new process.** Redo the whole flow
    on a fresh install rather than trying to continue a half-finished one.

---

## This repo's lessons
