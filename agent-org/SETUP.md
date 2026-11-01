# Agent organization installer

This file is the installer for the public, docs-only kit. A fresh agent can follow it in any target repository. The kit is tool-neutral until the tool-detection step.

## Rules before writing

- Work from the target repository root.
- Read this file, `agent-org/OPERATING.md`, `agent-org/ROUTER.md`, the decision register, the templates, and the role charters before proposing anything.
- Do not write files before the owner approves the roster. Roster approval is the only owner question in this installer.
- Use the target repository's existing conventions. Do not replace existing instructions, decisions, state, lessons, or member charters.
- Keep CI to lint, build, and test. CI must not deploy or converge infrastructure.
- Never put names, handles, machine names, hosts, IPs, network identifiers, credentials, or absolute local paths in committed files.

## 1. Read the target repository

Inspect the repository before making a proposal. Record evidence in the installer's working notes, not in committed files yet.

1. Identify the repository root and read its `README`, `docs/`, package manifests, lockfiles, build files, test configuration, and CI workflows. Identify languages, runtimes, package managers, build commands, lint commands, test commands, service-composition files, container files, and release notes.
2. Read every existing `AGENTS.md` and `AGENTS.override.md` that applies at the repository root. Existing instructions outrank this kit. Do not overwrite them.
3. Detect the agent tool in use:
   - Codex: look for `.codex/` and Codex configuration or instructions.
   - Kiro: look for `.kiro/` and Kiro agent configuration.
   - OpenCode: look for `.opencode/`, `opencode.json`, or `opencode.jsonc`.
   - If more than one signal exists, treat all matching tools as in use and generate every matching adapter.
   - If none is detectable, ask the owner which tool they use or whether to use the generic `AGENTS.md` path. Do not ask any other owner question.
4. Note the files and commands that each selected role will own. If a role has no relevant surface, record that explicitly as “Nothing to own in this repository at install time.”

## 2. Propose the roster and wait

Show the owner the default roster, with one line per role describing what that role will own in this repository. Tailor each line to the evidence from step 1. Flag roles with nothing to own. Use this default roster unless the owner removes a role:

- **Product Owner** (`product-owner`): owns the product definition, open product forks, and the front door for this repository.
- **Architect** (`architect`): owns the boundaries, interfaces, and architecture records for this repository.
- **Engineering Manager** (`engineering-manager`): owns sequencing, work breakdown, and capacity for this repository.
- **DevOps Lead** (`devops-lead`): owns the development environment, compose parity, CI lint/build/test lanes, and release preparation for this repository.
- **Software Lead** (`software-lead`): owns implementation patterns and code-level conventions for this repository.
- **QA Lead** (`qa-lead`): owns acceptance criteria, test strategy, and independent verification for this repository.
- **UX / Design Lead** (`ux-design-lead`): owns interaction, accessibility, user-facing vocabulary, and assets for this repository.
- **Conductor** (`conductor`): owns Worker briefs, routing, evidence checks, and handoffs for this repository.
- **Worker** (`worker`): owns one bounded mechanical change at a time in this repository.

Wait for the owner's approval or requested removals. Do not create `agent-org/`, modify `AGENTS.md`, modify `.gitignore`, or generate native files until the owner responds. This is the only owner question. If the owner requests a roster change, apply it without reopening the tool or architecture decisions already recorded in the kit.

## 3. Copy the kit and tailor the members

After approval, let `KIT_ROOT` be the directory containing the `SETUP.md` that the agent is following. Let `TARGET_ROOT` be the repository root.

1. Create `TARGET_ROOT/agent-org/` and copy the public kit documents from `KIT_ROOT`: `SETUP.md`, `OPERATING.md`, `ROUTER.md`, `adapters/`, `roles/`, and `templates/`. These files explain the installed contract and remain committed.
2. Create `TARGET_ROOT/agent-org/members/`.
3. For each approved role, copy its matching `KIT_ROOT/agent-org/roles/<id>.md` to `TARGET_ROOT/agent-org/members/<id>.md`.
4. Fill every `<!-- INSTALLER: ... -->` placeholder in each new member charter from the evidence collected in step 1. Name actual repository-relative documents, commands, source boundaries, CI files, and validation paths. Keep placeholders out of the installed member files. If the role has no surface, write that fact in its `This repo` section instead of inventing ownership. Keep the role's “Use me when” quote and ownership boundaries unchanged.
5. Initialize these files only when they do not already exist:
   - `agent-org/decision-register.md` from `templates/decision-register.md`.
   - `agent-org/needs-human.md` from `templates/needs-human.md`.
   - `agent-org/state-of-play.md` from `templates/state-of-play.md`.
   - `agent-org/lessons.md` from `templates/lessons.md`.
6. Do not replace an existing decision register, needs-human queue, state-of-play file, lessons file, or filled member charter. Preserve every status tag and every existing row. The installer may add a dated install note only when the repository's own workflow requires one; it must not promote an OPEN or SUGGESTED row.

## 4. Add the session ignore entry

Append `.agent-org-session/` to the repository's existing `.gitignore` if that exact entry is absent. Create `.gitignore` only if it does not exist. Preserve all existing patterns and comments. Do not ignore committed `agent-org/` files.

## 5. Add the router block to AGENTS.md

If `AGENTS.md` is missing, create it with the repository's normal heading followed by the block below. If it exists, append the block without changing existing content. Add the block once only.

```markdown
<!-- agent-org:begin -->
## Agent organization
Before any work, read `agent-org/ROUTER.md`. It selects the Product Owner or the direct member named by the user. Then follow `agent-org/OPERATING.md` and the selected `agent-org/members/<id>.md` charter. Use `agent-org/state-of-play.md`, `agent-org/needs-human.md`, and the relevant rows in `agent-org/decision-register.md` as shared state.
<!-- agent-org:end -->
```

The block is intentionally short. It points at the source files instead of copying their contents. If an existing `AGENTS.md` tells the tool to read another instruction file first, preserve that order and place this block where the repository's instructions allow it.

## 6. Generate native agent files

Generate native files only for tools detected in step 1. Use the matching adapter in `agent-org/adapters/` as the format authority. Generate one file per approved member, including the Worker. Use the exact first quote line in each role charter as the native `description`.

Before writing model fields, inspect the selected tool's locally offered models without signing in, starting a prompt, or writing to a global configuration directory. Fill `__TOP_MODEL__` for every thinking member and `__CHEAP_MODEL__` for the Worker. If a model cannot be confirmed from the tool's offer, stop native generation and report the missing evidence instead of guessing.

- **Codex:** write `.codex/agents/<id>.toml` using `adapters/codex.md`. Keep thinking agents away from shell tools where the documented feature allows it. Do not write to `~/.codex`.
- **Kiro CLI 2.x:** write `.kiro/agents/<id>.json` using the legacy documented fields in `adapters/kiro.md`. Validate each file with the installed Kiro CLI. Do not write to `~/.kiro`.
- **Kiro CLI V3:** write `.kiro/agents/<id>.md` using the current frontmatter format in `adapters/kiro.md`. Use `permissions` rather than `toolsSettings` or `allowedTools`.
- **OpenCode:** write `.opencode/agents/<id>.md` using `adapters/opencode.md`. If an existing config defines the same agent name, stop and report the collision. Do not overwrite an existing agent without an explicit owner instruction.
- **Generic:** write no native file. The `AGENTS.md` block and `ROUTER.md` provide the portable path.

Native files must point at `agent-org/OPERATING.md` and `agent-org/members/<id>.md`. Do not paste charters into native files. Keep the top-level session responsible for spawning. Enforce one nesting level: the top-level session may start a member or Worker, and no child may start another child.

## 7. Self-check and report

Run the following checks and report each result with the paths and counts. A failed check blocks the install commit.

1. **Kit files:** `agent-org/SETUP.md`, `OPERATING.md`, `ROUTER.md`, `adapters/`, `roles/`, `templates/`, and the approved `members/` files exist.
2. **Persistent files:** `agent-org/decision-register.md`, `needs-human.md`, `state-of-play.md`, and `lessons.md` exist. Existing files were not replaced.
3. **Member tailoring:** no `<!-- INSTALLER:` placeholder remains in an installed member charter, and every approved member has a `This repo` section.
4. **AGENTS block:** the `agent-org:begin` and `agent-org:end` markers occur exactly once and the block points to `agent-org/ROUTER.md`.
5. **Gitignore:** `.agent-org-session/` occurs as an entry in `.gitignore`, and `git status --short` does not show a session directory.
6. **Native coverage:** for every detected native tool, every approved member has exactly one native file in the correct directory and the file points to the member charter and operating contract.
7. **Native format:** parse each TOML, JSON, or YAML frontmatter file with a read-only local validator or the tool's documented validation command. Check descriptions, model pins, permissions, and Worker routing. Do not start an agent prompt.
8. **Hygiene:** run the repository's required identity-hygiene grep from the project instructions against `agent-org`, `docs`, `README.md`, and every changed file. It must print nothing. Also reject absolute local paths, credentials, hosts, IPs, handles, and machine identifiers in the changed files.
9. **Diff scope:** inspect `git diff --check` and `git diff --stat`. Only installer, kit, router, ignore, and approved native files may be changed.

## 8. Commit and open a PR in the target repository

Install on a branch, never on `main`.

1. Create a branch such as `agent-org/install` or `agent-org/upgrade` from the target repository's current base.
2. Run the self-check again after staging and before committing.
3. Commit the green result with the repository's configured identity. The commit message should say what was installed, for example `Install agent organization kit`.
4. Push the branch and open a pull request in the target repository. The PR should explain the detected tools, selected roster, generated native files, self-check results, and any known limits. The PR must not include deployment or infrastructure-convergence behavior.
5. The owner merges the PR. Do not push to `main`. Treat the install as complete only after the repository's lint, build, and test CI checks are green.

## 9. Re-run and upgrade

Run this installer again when a newer kit copy is available or the repository adds a supported tool.

1. Repeat the read and detection steps. Re-propose the roster only if the owner has not already approved it for this installation or the repository's owned surfaces changed materially.
2. Update the public kit files from the newer `KIT_ROOT`: `SETUP.md`, `OPERATING.md`, `ROUTER.md`, `adapters/`, `roles/`, and `templates/`.
3. Never overwrite the target's `agent-org/decision-register.md`, `state-of-play.md`, `needs-human.md`, or `lessons.md`.
4. Never overwrite an installed `agent-org/members/<id>.md` that has filled `This repo` content. Preserve local charter edits. If a new role was approved, create only that missing member file from the role template and fill its placeholders.
5. Regenerate native files from the preserved member charters. Add files for newly detected tools or members. Remove no native file automatically; report stale files for review.
6. Re-run every self-check, including the hygiene grep, and commit the upgrade on a branch. Open a PR rather than pushing to `main`.

An upgrade changes the kit and regenerates adapters. It does not erase the repository's decisions, state, open human questions, lessons, or filled member charters.
