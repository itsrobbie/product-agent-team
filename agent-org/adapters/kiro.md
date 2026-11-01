# Kiro adapter

Use this adapter when the target repository uses Kiro. The official references are [Kiro custom-agent configuration](https://kiro.dev/docs/custom-agents/configuration-reference/), [Kiro CLI 2.x reference](https://kiro.dev/docs/cli/2x-reference/), [Kiro sub-agents](https://kiro.dev/docs/custom-agents/subagents/), and [Kiro CLI V3 agent configuration](https://kiro.dev/docs/cli/v3/agent-config/).

## Native files and version selection

Kiro discovers project agents under `.kiro/agents/`. The installer reads `kiro-cli --version` and writes one native file per selected member:

- Kiro CLI 2.x: `.kiro/agents/<id>.json`.
- Kiro CLI V3: `.kiro/agents/<id>.md` with YAML frontmatter and a Markdown prompt body.

D9 is decisive. CLI 2.x does not load the Markdown agent files emitted for V3. V3 accepts the current JSON schema too, but this kit emits Markdown on V3 so the long prompt stays readable. The installer asks the active Kiro installation for available model ids and replaces `__TOP_MODEL__` and `__CHEAP_MODEL__` before writing. It must not commit an unavailable model id.

Paths in the examples are relative to `.kiro/agents/`. The `file://../../agent-org/...` URI reaches the repository root without an absolute machine path.

## CLI 2.x JSON format

Use the legacy fields documented for CLI 2.x. The confirmed fields used here are `name`, `description`, `prompt`, `model`, `tools`, `resources`, and `toolsSettings`. The `prompt` points at the member charter. `resources` includes the operating contract. `toolsSettings.subagent.availableAgents` and `trustedAgents` name only `worker`, so the top-level member may start a Worker but a Worker cannot start another agent. Thinking members list read and planning tools only. Worker gets the edit, shell, and test tools required by its brief. Do not substitute `allowedTools` for `tools`; it is not used by this adapter.

### Complete thinking-member example

`.kiro/agents/architect.json`:

```json
{
  "name": "architect",
  "description": "Use me when a decided product goal needs a coherent system design, clear boundaries, or a review of whether a proposed change fits the recorded architecture.",
  "prompt": "file://../../agent-org/members/architect.md",
  "model": "__TOP_MODEL__",
  "tools": ["fs_read", "grep", "glob", "code", "knowledge", "subagent"],
  "resources": ["file://../../agent-org/OPERATING.md", "file://../../agent-org/ROUTER.md"],
  "toolsSettings": {
    "subagent": {
      "availableAgents": ["worker"],
      "trustedAgents": ["worker"]
    }
  },
  "welcomeMessage": "Architect mode. Read the register and return design guidance or Worker briefs."
}
```

### Complete Worker example

`.kiro/agents/worker.json`:

```json
{
  "name": "worker",
  "description": "Use me when the Conductor has a single bounded mechanical work item with an explicit acceptance check, time budget, and hard stop.",
  "prompt": "file://../../agent-org/members/worker.md",
  "model": "__CHEAP_MODEL__",
  "tools": ["fs_read", "fs_write", "execute_bash", "grep", "glob", "code"],
  "resources": ["file://../../agent-org/OPERATING.md", "file://../../agent-org/ROUTER.md"],
  "welcomeMessage": "Worker mode. Read the brief, make one bounded change, and preserve evidence."
}
```

CLI 2.x field behavior varies by patch release. The installer must run the installed `kiro-cli agent validate` command on every generated JSON file and report failures instead of guessing a replacement field.

## CLI V3 Markdown format

V3 uses YAML frontmatter plus a Markdown system prompt. Current fields include `name`, `description`, `model`, `tools`, `excludedTools`, `resources`, and `permissions`. The examples use `permissions.rules` to deny edits and shell for thinking members. The rule matches use the documented capability names. A parent policy can still be stricter.

### Complete thinking-member example

`.kiro/agents/architect.md`:

```markdown
---
name: architect
description: Use me when a decided product goal needs a coherent system design, clear boundaries, or a review of whether a proposed change fits the recorded architecture.
model: __TOP_MODEL__
tools: ["read", "grep", "glob", "code", "knowledge", "subagent"]
excludedTools: ["write", "shell"]
resources:
  - file://../../agent-org/OPERATING.md
  - file://../../agent-org/ROUTER.md
  - file://../../agent-org/members/architect.md
permissions:
  rules:
    - capability: fs_write
      effect: deny
    - capability: shell
      effect: deny
welcomeMessage: Architect mode. Read the register and return design guidance or Worker briefs.
---

Read `agent-org/OPERATING.md` and `agent-org/members/architect.md` before acting. Read `agent-org/ROUTER.md` when the user asks to switch member context. Work only inside the Architect charter. Do not edit product source, run builds or tests, or start another agent. The top-level session owns delegation. Use one spawning level only.
```

### Complete Worker example

`.kiro/agents/worker.md`:

```markdown
---
name: worker
description: Use me when the Conductor has a single bounded mechanical work item with an explicit acceptance check, time budget, and hard stop.
model: __CHEAP_MODEL__
tools: ["read", "write", "shell", "grep", "glob", "code"]
resources:
  - file://../../agent-org/OPERATING.md
  - file://../../agent-org/ROUTER.md
  - file://../../agent-org/members/worker.md
welcomeMessage: Worker mode. Read the brief, make one bounded change, and preserve evidence.
---

Read `agent-org/OPERATING.md`, `agent-org/members/worker.md`, the assigned brief, and `agent-org/state-of-play.md` before acting. Execute exactly one bounded item. Do not decide policy and do not spawn another agent. Preserve evidence and report to the session that started you.
```

The installer validates Markdown frontmatter and model availability using the installed Kiro CLI. It must not run a prompt, sign in, or write to global Kiro directories.

## Spawning and direct mode

Kiro's sub-agent feature is used only by the top-level session. A thinking member may start a Worker in a top-level direct-mode session. A child never starts a child. For CLI 2.x, the available/trusted agent settings document this boundary. For V3, the `subagent` selector and permission rules are the documented controls, but exact parent-child behavior can vary with the installed release. The prompt contract remains authoritative.

Direct mode is selected by the router phrases in `agent-org/ROUTER.md`. Kiro's native agent picker or agent swap can be used as a shortcut after the matching member file is loaded. Switching back to the Product Owner reloads the router and the shared state files.

## Known limits

- CLI 2.x and V3 do not share one file format. Emit JSON for 2.x and Markdown for V3.
- The CLI 2.x reference documents `tools` and `toolsSettings` for agent profiles. The installed 2.37 validator accepted the adapter's `tools`/`toolsSettings` shape and rejected a scratch profile using only `allowedTools`; do not generate `allowedTools`.
- CLI V3 ignores `toolsSettings` and legacy `allowedTools` until migration. Do not put those fields in V3 Markdown.
- Kiro validates whether the pinned model is available when a session starts. An unavailable pin can make the first prompt fail. The installer must ask the local model list rather than guess.
- V3 permissions are capability rules, while `tools` selectors control visibility. They are not interchangeable.
- Kiro checks a child agent against the parent policy too. An agent's allow rule cannot grant access the parent denied.
- The docs do not state a universal hard numeric nesting limit for every Kiro surface. The kit enforces one level through available-agent configuration where supported and through instructions everywhere.
