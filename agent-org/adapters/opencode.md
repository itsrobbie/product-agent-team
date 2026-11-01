# OpenCode adapter

Use this adapter when the target repository uses OpenCode. The official reference is [OpenCode agents](https://opencode.ai/docs/agents/).

## Native files and generation

Project-scoped agents live in `.opencode/agents/`. The installer writes one Markdown file named `.opencode/agents/<id>.md` for every selected member. The filename is the native agent name. The Markdown charter remains the source of truth. The prompt points at `agent-org/members/<id>.md` and `agent-org/OPERATING.md` rather than copying either file.

The installer asks OpenCode which models it offers, using the local installation's documented model listing command. It replaces `__TOP_MODEL__` and `__CHEAP_MODEL__` with offered provider/model ids. It must not commit a model id that the local installation does not report.

## File format

OpenCode Markdown agents use YAML frontmatter. The documented fields used by this adapter are `description`, `mode`, `model`, `prompt`, `permission`, and `hidden`. `mode: subagent` is used for members invoked by the top-level session. The Product Owner may be emitted as `mode: primary` when the installer is configuring the main front door; all other members are subagents. If a repository already has a primary agent policy, preserve it and add the kit members as subagents.

### Complete thinking-member example

`.opencode/agents/architect.md`:

```markdown
---
description: Use me when a decided product goal needs a coherent system design, clear boundaries, or a review of whether a proposed change fits the recorded architecture.
mode: subagent
model: __TOP_MODEL__
prompt: "Read {file:../../agent-org/OPERATING.md} and {file:../../agent-org/members/architect.md} before acting. Read {file:../../agent-org/ROUTER.md} when the user asks to switch member context."
permission:
  edit: deny
  bash: deny
  task:
    "*": deny
    worker: allow
---

Work only inside the Architect charter. Return design decisions and Worker briefs to the top-level session. Do not edit product source, run builds or tests, or start a child of a child. Record role-owned decisions and open threads in the shared files as the operating contract requires.
```

### Complete Worker example

`.opencode/agents/worker.md`:

```markdown
---
description: Use me when the Conductor has a single bounded mechanical work item with an explicit acceptance check, time budget, and hard stop.
mode: subagent
model: __CHEAP_MODEL__
prompt: "Read {file:../../agent-org/OPERATING.md} and {file:../../agent-org/members/worker.md} before acting. Read the assigned brief and {file:../../agent-org/state-of-play.md}."
permission:
  task: deny
---

Execute exactly one bounded work item. Do not decide product intent, architecture, implementation policy, or quality policy. Do not spawn another agent. Preserve evidence, commit after a green step, and report the exact revision, commands, results, and limitations to the session that started you.
```

For the Product Owner, the installer uses the charter's exact first quote line as `description`, `mode: primary`, the top model, and a task permission that allows only the selected Worker or approved member names. For thinking leads, `edit: deny` and `bash: deny` keep them off file-edit and shell tools. OpenCode's permissions are evaluated with last-match-wins behavior; the installer must place the wildcard before exceptions.

## Spawning and direct mode

OpenCode has primary agents and subagents. The top-level Product Owner is the primary front door. It invokes leads and Workers through the Task tool. A lead in top-level direct mode may invoke `worker`, but a child agent has `task: deny` and may not start another agent. This is the kit's one-level nesting rule.

Native direct mode maps as follows:

- `Tab` switches among primary agents. The installer makes the Product Owner primary and does not silently convert an existing primary agent.
- `@architect`, `@qa-lead`, and similar mentions invoke a named subagent directly.
- Phrases such as `I'm talking to the Architect` remain router instructions and load the same charter and shared state files. They are the portable path and must work even when native shortcuts are unavailable.

## Known limits

- OpenCode supports both `opencode.json` agents and Markdown agents. This adapter uses project Markdown files so each member is independently reviewable.
- The documented `prompt` file reference is relative to the config file. The installer validates the relative paths from `.opencode/agents/`.
- OpenCode's `permission` field gates tools, but a top-level parent policy can still ask or deny a child operation.
- `edit: deny` and `bash: deny` are documented for thinking agents. OpenCode does not document a separate distinction between every possible file-edit mechanism beyond the `edit` permission group.
- Task permissions control which subagents are exposed to the model, while direct `@` invocation remains user-visible. The router and review rules still enforce the one-level contract.
- If an existing `opencode.json` defines the same agent name, the installer must stop and report the collision instead of silently replacing it.
