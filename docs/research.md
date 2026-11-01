# Research: how leading teams run multi-agent setups (November 2026)

## 1. What they do

| Who | Pattern | What it tells us |
|---|---|---|
| **Anthropic, Research feature** ([post](https://www.anthropic.com/research/multi-agent-research-system)) | One lead agent plans, saves its plan to memory, spawns parallel subagents in their own context windows, then synthesises. An Opus lead with Sonnet workers beat single-agent Opus by 90.2% on their research eval. | Orchestrator-worker with an expensive lead and cheap workers works. It costs about 15x the tokens of a chat. Coding has fewer truly parallel tasks than research. |
| **Anthropic, long-running harnesses** ([harnesses](https://www.anthropic.com/research/effective-harnesses-for-long-running-agents), [harness design](https://www.anthropic.com/engineering/harness-design-long-running-apps)) | Planner, generator and evaluator agents. Each session starts from a structured handoff file, not memory. A separate, deliberately skeptical evaluator grades the work. | Context resets with a handoff file beat long sessions. An agent grading its own work praises it, so the verifier must be a different agent. |
| **Cognition / Devin** ([2025](https://cognition.ai/blog/dont-build-multi-agents), [2026 update](https://cognition.ai/blog/multi-agents-working)) | Was against multi-agent. Now ships multi-agent where many agents contribute intelligence but writes stay single-threaded: a coder plus a clean-context reviewer loop, and a cheap primary model that consults a "smart friend" model. | Parallel writers make conflicting implicit decisions. Share plan and decision files with every agent. Reviewers work best with no shared context. |
| **OpenAI Agents SDK** ([orchestration](https://openai.github.io/openai-agents-python/multi_agent/)) | Two named patterns: agents-as-tools (a manager keeps the conversation and calls specialists) and handoffs (a specialist takes over the conversation). | Talking to the Product Owner is agents-as-tools; "I'm talking to the Architect" is a handoff. |
| **Claude Code** (subagents in `.claude/agents/*.md`) | Markdown files with YAML frontmatter define each subagent. Nested spawning arrived in 2026 and is capped at depth 3 by default after runaway-token incidents. | Markdown agent definitions are the de facto format. Depth limits are real and tool-specific. |
| **OpenAI Codex** (subagents GA March 2026) | Custom agents are TOML files in `.codex/agents/`. Defaults: `max_threads` 6, `max_depth` 1, so a child cannot spawn grandchildren. | A portable kit cannot assume nesting. |
| **AGENTS.md** standard | Read by Codex, Copilot, Cursor, Gemini/Jules, Windsurf, Zed, Amp, Factory, Kiro, OpenCode and others. | AGENTS.md is the one entry point nearly every tool honours. |
| **Google ADK** | A coordinator with `sub_agents` delegates based on each sub-agent's description field. | Routing quality depends on clear "when to use me" descriptions. |
| **BMAD Method** (open source) | Pure-markdown personas (Analyst, PM, Architect, PO, Scrum Master, Dev, QA, Orchestrator), each phase producing committed markdown artefacts. | The closest existing thing. It is a phase pipeline with no decision register, escalation gate or single front door. |

## 2. How this setup compares

This kit generalises an agent org that has been running a real product build on Kiro Crew: a
Product Owner front door, seven leads, a Conductor and a cheap Worker pool.

**Ahead of common practice.** No mainstream framework ships a decision register with
DECIDED/SUGGESTED/OPEN tags, an escalation gate, or a rule that only one agent talks to the human.
Those are the parts that stop agents re-asking the human, and they are what the kit carries.

**In line with common practice.** Orchestrator-worker; expensive leads with cheap workers; QA as a
separate verifier ("a done message is not proof"); durable handoff files; a branch or worktree per
parallel writer.

**Riskier than common practice, and how the kit responds.**
- **Depth.** The reference org runs 3 to 4 levels. Industry runs 1 to 2. The kit keeps the tiers as
  roles but lets only the top-level session start agents.
- **Parallel writers.** One integrator owns merges; parallel Workers write on separate branches.
- **No baseline.** The reference org never measured itself against a single agent. The kit's
  done-criteria require it.
- **Reliability lived in the runtime.** Overrunning leads, early stops and orphaned work were caught
  by background monitors. A markdown kit has none, so the rules move into every brief: one
  deliverable, a time budget with a hard stop, commit after every green step.
- **Lessons lived in the runtime.** About 30 hard-won corrections sat in agent memory. The kit makes
  them a committed `lessons.md`.

## 3. The three target tools

| | Codex | Kiro (IDE 1.0 / CLI V3) | OpenCode |
|---|---|---|---|
| Reads AGENTS.md | Yes, layered global and project | Yes, root, nested and `~/.kiro/steering/` | Yes (Rules) |
| Native agent files | `.codex/agents/*.toml` | `.kiro/agents/` as Markdown or JSON | `.opencode/agents/*.md` with frontmatter |
| Per-agent model pin | Yes (`model`, `model_reasoning_effort`) | Yes | Yes (`model`) |
| Subagents | Yes, when asked; 6 parallel by default | Yes, by description; parallel; task graphs and review loops | Yes, by description or @mention; parallel child sessions |
| Nesting | No grandchildren by default | Not documented | Not documented |
| Native persona switch | None; router text does it | Agent swap | Tab between primary agents, @mention |

Sources: [Codex subagents](https://developers.openai.com/codex/concepts/subagents),
[Codex AGENTS.md](https://developers.openai.com/codex/guides/agents-md),
[Kiro sub-agents](https://kiro.dev/docs/custom-agents/subagents/),
[Kiro CLI V3 agent config](https://kiro.dev/docs/cli/v3/agent-config/),
[Kiro nested AGENTS.md](https://kiro.dev/changelog/cli/2-18/),
[OpenCode agents](https://opencode.ai/docs/agents/).
