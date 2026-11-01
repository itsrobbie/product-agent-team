# Product Agent Team

A drop-in, markdown-only setup for running a software product with a team of AI agents, where you
talk to **one** agent (the Product Owner) and it decides, delegates and shields you from everything
that does not genuinely need a human.

It is docs only and tool-neutral. There are no scripts, adapters or tool files: you point the agent
in a repo at `agent-org/SETUP.md`, it sets the team up from these docs, and it wires the team into
its own tool (subagents, models, permissions) however that tool allows.

## Use it

Copy `agent-org/` somewhere the target repo's agent can read it, then tell that agent:
"Follow `agent-org/SETUP.md` to set up the product agent team in this repo with the default
roster." After the PR merges, talk to the Product Owner, or say "I'm talking to the QA Lead" to
work with one member directly.

## Why

Flat agent setups keep asking the human things that are already decided, or that another agent
could answer. This setup fixes that with:

- **One front door.** You talk to the Product Owner. It brings you only human-only forks, each with
  a recommendation.
- **Decide-then-record.** A decision does not exist until it is a row in a decision register
  tagged DECIDED, SUGGESTED or OPEN. Nobody re-asks a DECIDED item.
- **An escalation gate.** Check the register, then the peer who owns the answer, and only then the
  human.
- **Think on the top model, do on a cheap model.** Leads read, plan, decide and review. Cheap
  Workers make every edit and run every build and test.
- **Direct mode.** Say "I'm talking to the Architect" and the chat becomes that member, with its
  charter and the shared state loaded, until you say "back to the Product Owner".

## Read

- [`agent-org/`](agent-org/): the kit itself (`SETUP.md`, `OPERATING.md`, `ROUTER.md`, `roles/`,
  `templates/`).
- [`docs/research.md`](docs/research.md): how leading teams (Anthropic, Cognition, OpenAI, Google,
  the AGENTS.md tools, BMAD) build multi-agent systems, and how this setup compares.
- [`docs/definition.md`](docs/definition.md): what the kit is, what it is not, and what done means.
- [`docs/decisions.md`](docs/decisions.md): this project's own decision register.

## Licence

[MIT](LICENSE).
