# Setup

Point the agent in a target repository at this file. It tells that agent what to set up. How the
team is wired into the agent's own tool (subagents, models, permissions, agent pickers) is left to
that environment: use whatever features it has, guided by the principles at the end.

## Rules

- Work from the target repository root.
- Read this file, `OPERATING.md`, `ROUTER.md`, the role charters in `roles/` and the templates in
  `templates/` before proposing anything.
- The target repository's existing instructions outrank this kit. Never overwrite them.
- Write nothing until the owner approves the roster. That is the only question to ask.
- Never commit names, handles, machine names, hostnames, network identifiers, credentials or
  absolute local paths.

## 1. Read the repository

Read the README, `docs/`, build and test configuration, CI workflows and any existing `AGENTS.md`.
Note what each role would own here: the languages, build and test commands, CI files, where design
records live, where product docs live. If a role has nothing to own, note that.

## 2. Propose the roster

Show the owner the default roster, one line per role saying what it will own in this repository,
and flag any role with nothing to own:

- **Product Owner** (`product-owner`): product definition, open product forks, the front door.
- **Architect** (`architect`): boundaries, interfaces, architecture records.
- **Engineering Manager** (`engineering-manager`): sequencing, work breakdown, capacity.
- **DevOps Lead** (`devops-lead`): environments, CI, release automation.
- **Software Lead** (`software-lead`): implementation patterns and code conventions.
- **QA Lead** (`qa-lead`): acceptance criteria, test strategy, independent verification.
- **UX / Design Lead** (`ux-design-lead`): interaction, accessibility, user-facing wording, assets.
- **Conductor** (`conductor`): Worker briefs, routing, checking Workers' evidence.
- **Worker** (`worker`): one bounded hands-on change at a time.

Wait for approval or removals. If the message that asked for the setup already approves a roster
(for example "set up the team with the default roster"), treat that as the approval and do not ask
again.

## 3. Copy and tailor

1. Copy this kit into the target as `agent-org/`: `SETUP.md`, `OPERATING.md`, `ROUTER.md`,
   `roles/` and `templates/`.
2. For each approved role, copy `roles/<id>.md` to `agent-org/members/<id>.md` and fill its
   `This repo` section from what step 1 found. Use real repo-relative paths and commands. If the
   role has nothing to own, say so rather than inventing ownership.
3. Create these from `templates/`, only if they do not already exist:
   `agent-org/decision-register.md`, `agent-org/needs-human.md`, `agent-org/state-of-play.md`,
   `agent-org/lessons.md`.
4. Add `.agent-org-session/` to `.gitignore`.
5. Add this block to `AGENTS.md` (create the file if missing; append, never replace):

```markdown
<!-- agent-org:begin -->
## Agent organization
Before any work, read `agent-org/ROUTER.md`. It selects the Product Owner or the member the user
names. Then follow `agent-org/OPERATING.md` and that member's `agent-org/members/<id>.md`.
<!-- agent-org:end -->
```

If the environment's tool reads a different instruction file instead of `AGENTS.md`, put the same
block there too.

## 4. Wire it into your environment

This kit ships no tool files. Use your own tool's features to honour these principles, in whatever
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

## 5. Check and hand over

Before committing, confirm:

- every approved member has a filled `agent-org/members/<id>.md` with no placeholder left;
- the register, needs-human, state-of-play and lessons files exist, and existing ones were not
  replaced;
- the `AGENTS.md` block appears exactly once;
- `.agent-org-session/` is ignored;
- no names, handles, hostnames, local paths or credentials appear in the changed files.

Then commit on a branch, push it, and open a pull request (`OPERATING.md` section 9). The owner
merges it.

## Updating

To update an existing install from a newer copy of this kit, replace `SETUP.md`, `OPERATING.md`,
`ROUTER.md`, `roles/` and `templates/`. Never overwrite the repository's register, state-of-play,
needs-human, lessons or filled member charters. Add a member file only for a newly approved role.
