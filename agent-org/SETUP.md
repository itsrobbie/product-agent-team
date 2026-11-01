# Setup

Point the agent in a target repository at this file. It tells that agent what to set up. Most of
the time the target is an existing repository with its own history, conventions, decisions and
work in progress; this file is written for that case. A brand-new repository is the easy version of
the same steps.

How the team is wired into the agent's own tool (subagents, models, permissions, agent pickers) is
left to that environment: use whatever features it has, guided by section 5.

## Rules

- Work from the target repository root.
- Read this file, `OPERATING.md`, `ROUTER.md`, the role charters in `roles/` and the templates in
  `templates/` before proposing anything.
- **The repository wins.** Its existing instructions, conventions, decision records and processes
  outrank this kit. The kit fits around them; it never replaces, moves or rewrites them.
- **Leave work in progress alone.** Do not touch uncommitted changes, open branches or open PRs.
  Set up on a new branch from the default branch.
- Write nothing until the owner approves the proposal in step 3. That is the only question to ask.
- Never commit names, handles, machine names, hostnames, network identifiers, credentials or
  absolute local paths.

## 1. Read the repository

Read enough to know how this repository already works:

- **What it is:** README, product docs, roadmap, changelog.
- **How it is built:** languages, build, lint and test commands, CI workflows, release process.
- **How work flows:** branch naming, commit style, PR template, CODEOWNERS, review rules, the issue
  tracker, labels, milestones.
- **What is already decided:** ADRs, RFCs, design docs, decision logs, "why" sections, accepted
  PR discussions linked from docs.
- **What is in flight:** open PRs, active branches, open issues or TODO lists the docs point to.
- **Existing agent instructions:** `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`,
  `.github/copilot-instructions.md`, `.cursor/rules`, `.kiro/steering`, and any agent or persona
  definitions already in the repo.
- **People:** who owns what today (CODEOWNERS, maintainers listed in docs). The team's roles sit
  alongside them and never replace a human reviewer or approver.

## 2. Map the kit onto what exists

Before proposing anything, decide how each kit file relates to what the repository already has.
Prefer pointing at existing things over creating parallel ones.

| Kit file | If the repository already has an equivalent |
|---|---|
| `decision-register.md` | Do not copy or move existing decisions. Create the register as an **index**: one DECIDED row per existing decision record, with a one-line summary and a link to the source. New decisions follow the repo's existing format (for example, a new ADR) and get an index row. |
| `state-of-play.md` | Seed it from the current state: the active goal if the docs state one, open PRs and branches that matter, and known blockers. Link to the issue tracker rather than copying issues. |
| `needs-human.md` | If the repo already has a place for questions to maintainers (a label, a discussion category), note it there and keep this file as the agents' queue, linking each row to that place when one is used. |
| `lessons.md` | Start from the template. Add any standing rules the existing agent instructions already state, so members see them in one place, and link to the source. |
| member charters | Fill each `This repo` section with the real paths, commands, conventions and human owners found in step 1. |
| `OPERATING.md` rules | Where a repo convention differs (branch names, commit style, how PRs are reviewed, who merges), the repo convention applies. Record each difference as a DECIDED register row so members do not guess. |

Also check for:

- **Conflicts with existing agent instructions.** If an existing instruction contradicts the kit
  (for example, "commit directly to main"), the existing instruction wins. List each conflict.
- **Existing agent or persona definitions.** Keep them. Note which kit member each one overlaps
  with, if any.
- **Name collisions.** If `agent-org/` already exists and is not a previous install of this kit,
  use `docs/agent-org/` instead and write that path everywhere the kit says `agent-org/`.

## 3. Propose and wait

Show the owner one proposal:

1. The roster, one line per role saying what it will own in this repository and which human owners
   it works alongside, flagging any role with nothing to own:
   - **Product Owner** (`product-owner`): product definition, open product forks, the front door.
   - **Architect** (`architect`): boundaries, interfaces, architecture records.
   - **Engineering Manager** (`engineering-manager`): sequencing, work breakdown, capacity, Worker briefs and routing, checking Workers' evidence.
   - **DevOps Lead** (`devops-lead`): environments, CI, release automation.
   - **Software Lead** (`software-lead`): implementation patterns and code conventions.
   - **QA Lead** (`qa-lead`): acceptance criteria, test strategy, independent verification.
   - **UX / Design Lead** (`ux-design-lead`): interaction, accessibility, user-facing wording, assets.
   - **Worker** (`worker`): one bounded hands-on change at a time.
2. How the kit maps onto what exists (step 2), in a few lines: what will be indexed, what will be
   pointed at, and which repo conventions override kit defaults.
3. Any conflicts with existing agent instructions, and how each resolves.
4. The files the setup PR will add or change. It only adds files under `agent-org/`, appends to
   `AGENTS.md` and `.gitignore`, and changes nothing else.

Wait for approval or changes. If the message that asked for the setup already approves (for
example "set up the team with the default roster"), treat that as approval of the roster, still
show the rest of the proposal, and only stop if step 2 found a conflict the owner has not seen.

## 4. Write

1. Copy this kit into the target as `agent-org/`: `SETUP.md`, `OPERATING.md`, `ROUTER.md`,
   `roles/` and `templates/`.
2. For each approved role, copy `roles/<id>.md` to `agent-org/members/<id>.md` and fill its
   `This repo` section from steps 1 and 2.
3. Create `agent-org/decision-register.md`, `agent-org/needs-human.md`,
   `agent-org/state-of-play.md` and `agent-org/lessons.md` from `templates/`, seeded as step 2 says.
   If any already exists from an earlier install, keep it.
4. Add `.agent-org-session/` to `.gitignore`.
5. Append this block to `AGENTS.md`, creating the file only if it does not exist. Never edit the
   existing content:

```markdown
<!-- agent-org:begin -->
## Agent organization
Before any work, read `agent-org/ROUTER.md`. It selects the Product Owner or the member the user
names. Then follow `agent-org/OPERATING.md` and that member's `agent-org/members/<id>.md`.
Where this repository's own instructions above differ from the agent organization, these
repository instructions win.
<!-- agent-org:end -->
```

If the repository's agents read a different instruction file (for example `CLAUDE.md`), append the
same block there too.
## 5. Wire it into your environment

This kit ships no tool files. If the repository already defines agents for its tool, extend that
setup rather than starting a second one. Use your own tool's features to honour these principles, in whatever
way it supports. If it supports none of them, the docs alone still work: the router and the rules
run the team inside one chat.

- **One member per agent definition.** If the tool lets you define named agents or subagents, define
  one per member. Each definition should point at `agent-org/OPERATING.md` and
  `agent-org/members/<id>.md` rather than copying them, and use the first quoted line of the charter
  as its "when to use me" description.
- **Think on the top model, do on a cheap model** (`OPERATING.md` section 5). If the tool lets you
  pick a model per agent, put thinking members on the strongest model and the Worker on a cheap one.
  If it lets you limit tools per agent, keep thinking members off file editing and shell.
- **Only the top-level session starts agents** (`OPERATING.md` section 6). Do not rely on subagents
  starting their own subagents.
- **Direct mode** (`ROUTER.md`). If the tool has its own agent picker, it should load the same
  member files the router does.

Keep any tool files you create out of `agent-org/`. They belong to the environment, not the kit.

## 6. Check and hand over

Before committing, confirm:

- every approved member has a filled `agent-org/members/<id>.md` with no placeholder left;
- the register, needs-human, state-of-play and lessons files exist, and existing ones were not
  replaced;
- the `AGENTS.md` block appears exactly once;
- `.agent-org-session/` is ignored;
- every existing decision record has an index row, and nothing was copied or moved;
- the diff only adds files under `agent-org/` and appends to `AGENTS.md` (or the repo's
  instruction file) and `.gitignore`; no existing file was otherwise changed;
- no pre-existing uncommitted change is in the commit;
- no names, handles, hostnames, local paths or credentials appear in the changed files.

Then commit on a new branch from the default branch, using the repository's branch and commit
conventions, push it, and open a pull request with the repository's PR template if it has one
(`OPERATING.md` section 9). The PR description repeats the step 3 proposal. The owner merges it.

## 7. First session after the merge

The first time the Product Owner runs, it reads the seeded `state-of-play.md` and the register
index, then gives the owner a short summary of what it understands: what the product is, what is
in flight, what is decided, and anything it could not work out. The owner corrects it once; the
corrections go to the register and `lessons.md`.

## Updating

To update an existing install from a newer copy of this kit, replace `SETUP.md`, `OPERATING.md`,
`ROUTER.md`, `roles/` and `templates/`. Never overwrite the repository's register, state-of-play,
needs-human, lessons or filled member charters. Add a member file only for a newly approved role.
Re-run step 2 to index any decision records added since the last update.
