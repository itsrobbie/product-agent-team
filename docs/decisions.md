# Decision register

A decision does not exist until it is a row here. Tags: **DECIDED** (settled, do not re-ask),
**SUGGESTED** (recommended, awaiting confirmation), **OPEN** (unresolved). Never promote OPEN to
DECIDED silently; supersede rows rather than deleting them.

| # | Decision | Tag | Date |
|---|---|---|---|
| D1 | The kit lives in its own public, docs-only repo, copied into target repos as `agent-org/`. | DECIDED | 2026-11-02 |
| D2 | Target tools: Codex, Kiro and OpenCode. Other AGENTS.md tools are covered by a generic adapter. | DECIDED | 2026-11-02 |
| D3 | Markdown is the source of truth; the installer also generates each tool's native agent files (Codex TOML, Kiro and OpenCode markdown). | DECIDED | 2026-11-02 |
| D4 | Member memory is session-scoped and gitignored; a short end-of-session summary goes to the register and `state-of-play.md`. | DECIDED | 2026-11-02 |
| D5 | Default roster: Product Owner, Architect, Engineering Manager, DevOps Lead, Software Lead, QA Lead, UX/Design Lead, Conductor, Worker. | DECIDED | 2026-11-02 |
| D6 | Think on the top model, do on a cheap model: only Workers edit files or run builds and tests. | DECIDED | 2026-11-02 |
| D7 | One level of spawning: only the top-level session starts agents. | DECIDED | 2026-11-02 |
| D8 | Licence: MIT, so the files can be copied into any repo. | SUGGESTED | 2026-11-02 |
