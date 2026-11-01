# Generic AGENTS.md adapter

Use this adapter for any tool that reads repository `AGENTS.md` instructions but has no documented native agent-file format.

## Native files and generation

There are no native agent files. The installer relies on the root `AGENTS.md` block and the portable router. It must not invent a tool-specific config file. The block tells every session to read `agent-org/ROUTER.md` first. The router then selects the Product Owner or a direct member and points at `agent-org/OPERATING.md`, `agent-org/members/<id>.md`, `agent-org/state-of-play.md`, `agent-org/needs-human.md`, and the relevant decision-register rows.

The generic path uses no model fields. Model choice is a tool or account setting outside this kit. The roster still keeps thinking work with the thinking members and mechanical work with the cheap Worker by contract.

## Complete worked examples

A root `AGENTS.md` block:

```markdown
<!-- agent-org:begin -->
## Agent organization
Before any work, read `agent-org/ROUTER.md`. It selects the Product Owner or the direct member named by the user. Then follow `agent-org/OPERATING.md` and the selected `agent-org/members/<id>.md` charter. Use `agent-org/state-of-play.md`, `agent-org/needs-human.md`, and the relevant rows in `agent-org/decision-register.md` as shared state.
<!-- agent-org:end -->
```

A thinking-member handoff in a chat:

```text
I'm talking to the Architect. Read agent-org/OPERATING.md and agent-org/members/architect.md. Return a design decision or a Worker brief. Do not edit source files or run tests.
```

A Worker handoff in a chat:

```text
Start the Worker for this one bounded brief. Read agent-org/OPERATING.md, agent-org/members/worker.md, the brief, and agent-org/state-of-play.md. Make only the named mechanical change, run its acceptance check, commit after a green step, and report the revision and evidence.
```

## Spawning and direct mode

The generic path has no native subagent control. Run Workers sequentially in the same conversation. After each Worker finishes, write its result and next step to `agent-org/state-of-play.md`, then reset the conversation context before starting the next Worker. The reset prevents one Worker's assumptions from leaking into the next one.

The top-level session is the Product Owner unless the user explicitly switches to a member. A direct member is still top-level only when the user addressed it directly. No child session may spawn another member. If the generic tool offers a separate session feature but no documented agent-file control, treat it as generic and keep the one-level rule in the chat instructions.

Direct mode is the portable phrases in `agent-org/ROUTER.md`: `I'm talking to <member>`, `@<member>`, and `switch to <member>`. `back to the Product Owner` ends direct mode. There is no native switching shortcut to document.

## Known limits

- This adapter cannot restrict tools, pin models, or enforce nesting. The chat contract and human/tool permissions remain the control surface.
- Sequential Workers are slower than native subagents, but they keep writes single-threaded and make state resets explicit.
- A generic tool may have its own instruction-file discovery rules. The installer verifies that the root `AGENTS.md` block is loaded only through the tool's documented read path; it does not assume nested or global discovery.
- Do not add a made-up native file. If the tool later documents one, add a dedicated adapter rather than changing this generic path.
