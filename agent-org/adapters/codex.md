# Codex adapter

Use this adapter when the target repository uses Codex. The official references are [Codex subagents](https://developers.openai.com/codex/concepts/subagents), [Codex custom agents](https://developers.openai.com/codex/agent-configuration/subagents), [Codex configuration reference](https://developers.openai.com/codex/config-reference/), and [Codex project instructions](https://developers.openai.com/codex/guides/agents-md). If a linked page moves, use the current Codex documentation for the same topic.

## Native files and generation

Project-scoped custom agents live in `.codex/agents/`. Each member gets one TOML file named `<id>.toml`. The installer writes these files from the member ids selected by the owner. The Markdown charter remains the source of truth. Do not paste the charter into TOML. Point the developer instructions at `agent-org/members/<id>.md` and `agent-org/OPERATING.md`.

The installer first asks the local Codex installation which models it offers. It replaces `__TOP_MODEL__` and `__CHEAP_MODEL__` with offered model ids. It must not commit a model id that the local installation does not report.

## File format

Every file uses the documented standalone-agent fields `name`, `description`, and `developer_instructions`. `model` and `model_reasoning_effort` are optional documented fields used here for the tier split. The `features.shell_tool = false` setting is used for thinking members. Codex does not document a separate per-agent file-write switch, so file-edit access cannot be narrowed further in this adapter. Parent-session sandbox and approval settings still apply.

### Complete thinking-member example

`.codex/agents/architect.toml`:

```toml
name = "architect"
description = "Use me when a decided product goal needs a coherent system design, clear boundaries, or a review of whether a proposed change fits the recorded architecture."
model = "__TOP_MODEL__"
model_reasoning_effort = "high"
features.shell_tool = false
developer_instructions = """
You are the Architect for this repository.

Before acting, read agent-org/OPERATING.md and agent-org/members/architect.md. Treat those files as the contract. Read agent-org/ROUTER.md when the user asks to change member context. Work only inside the Architect charter. Do not edit product source, run builds, run tests, or start another agent. Return design decisions and Worker briefs to the top-level session. Record role-owned decisions in agent-org/decision-register.md and open threads in agent-org/state-of-play.md as the contract requires.

The Product Owner is the front door. The top-level session owns delegation. Use one spawning level only. A Worker may not spawn a child.
"""
```

### Complete Worker example

`.codex/agents/worker.toml`:

```toml
name = "worker"
description = "Use me when the Conductor has a single bounded mechanical work item with an explicit acceptance check, time budget, and hard stop."
model = "__CHEAP_MODEL__"
model_reasoning_effort = "medium"
developer_instructions = """
You are the Worker for this repository.

Before acting, read agent-org/OPERATING.md and agent-org/members/worker.md. Read the assigned brief and agent-org/state-of-play.md before changing anything. Execute exactly one bounded item. Do not decide product intent, architecture, implementation policy, or quality policy. Do not spawn another agent. Preserve evidence, commit after a green step, and report the exact revision, commands, results, and limitations to the session that started you.
"""
```

The installer substitutes the chosen model ids and emits the same shape for every selected member. Thinking members use `__TOP_MODEL__`; Worker uses `__CHEAP_MODEL__`. If the owner explicitly approves a mid-tier Worker for one brief, that is a brief-level override, not a native roster default.

## Spawning and direct mode

Codex starts subagents from the top-level conversation through its subagent workflow. The kit's contract permits one level only: the top-level session starts members or Workers, and a child never starts a child. The installer may set the documented `[agents]` concurrency limit in `.codex/config.toml` only when the repository already has a config and the owner approves that change. The current key is `max_concurrent_threads_per_session`; `max_threads` is its legacy alias. Codex does not document a `max_depth` nesting field.

There is no native member-switch shortcut that replaces the router. The phrases `I'm talking to the Architect`, `@architect`, and `switch to architect` are handled by `agent-org/ROUTER.md`. A Codex custom agent can be invoked by its name from the top-level workflow, but direct-mode context still comes from the router and member file.

## Known limits

- Codex custom-agent TOML discovery under project-scoped `.codex/agents/` is documented. The filename normally matches the `name` field, but `name` is the source of truth.
- Custom agents inherit the parent sandbox, approvals, provider, MCP servers, and skills. Their files cannot grant broader access.
- `sandbox_mode` is a session/config setting. A custom-agent file cannot override the parent sandbox mode.
- Codex documents disabling `features.shell_tool`, but it does not document an independent per-agent file-write switch. Thinking members therefore rely on the shell restriction plus the charter contract.
- Codex does not document a native `max_depth` or other hard nesting limit for custom agents. The one-level rule is enforced by instructions and review.
- Do not run a prompt during installation. Validate syntax and file placement locally, then let the repository's normal Codex session verify loading after the owner starts it.
