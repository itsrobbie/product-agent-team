# Decision register

A decision does not exist until it is a row here. Tags: **DECIDED** (settled, do not re-ask),
**SUGGESTED** (recommended, awaiting confirmation), **OPEN** (unresolved). Never promote OPEN to
DECIDED silently; supersede rows rather than deleting them.

| # | Decision | Tag | Date |
|---|---|---|---|
| D1 | The kit lives in its own public, docs-only repo, copied into target repos as `agent-org/`. | DECIDED | 2026-11-02 |
| D2 | Target tools: Codex, Kiro and OpenCode. Other AGENTS.md tools are covered by a generic adapter. | SUPERSEDED by D17 | 2026-11-02 |
| D3 | Markdown is the source of truth; the installer also generates each tool's native agent files (Codex TOML, Kiro and OpenCode markdown). | SUPERSEDED by D17 | 2026-11-02 |
| D4 | Member memory is session-scoped and gitignored; a short end-of-session summary goes to the register and `state-of-play.md`. | DECIDED | 2026-11-02 |
| D5 | Default roster: Product Owner, Architect, Engineering Manager, DevOps Lead, Software Lead, QA Lead, UX/Design Lead, Conductor, Worker. | DECIDED | 2026-11-02 |
| D6 | Think on the top model, do on a cheap model: only Workers edit files or run builds and tests. | DECIDED | 2026-11-02 |
| D7 | One level of spawning: only the top-level session starts agents. | DECIDED | 2026-11-02 |
| D8 | Licence: MIT, so the files can be copied into any repo. | SUGGESTED | 2026-11-02 |
| D9 | Kiro adapter writes JSON agent files on Kiro CLI 2.x and Markdown on V3 (2.x does not load Markdown agent files). | SUPERSEDED by D17 | 2026-11-02 |
| D10 | Verification drives Codex and OpenCode through the CLIs bundled in their desktop apps, using the owner's existing sign-in; no separate CLI installs. | SUPERSEDED by D17 | 2026-11-02 |
| D11 | Design documents that are themselves the thinking (charters, ADRs, specs, kit docs) are written by the owning member; Workers do mechanical work only. | DECIDED | 2026-11-02 |
| D12 | One-level spawning is a kit contract enforced by orchestration and instructions, not by a native nesting setting (none is documented for Codex custom agents). | SUPERSEDED by D17 | 2026-11-02 |
| D13 | When no supported tool is detected, the installer falls back to the generic AGENTS.md-only adapter. | SUPERSEDED by D17 | 2026-11-02 |
| D14 | Kiro 2.x agent files use `tools` and `toolsSettings`, never `allowedTools` alone (rejected by the 2.37 validator); V3 files use `permissions`. OpenCode files use `permission`, not the deprecated `tools`. | SUPERSEDED by D17 | 2026-11-02 |
| D15 | The installer's identity-hygiene check builds its pattern at install time from local identity values and never commits the pattern. | DECIDED | 2026-11-02 |
| D16 | A roster approval given in the message that starts the setup counts as the approval; the setup does not ask again. | DECIDED | 2026-11-02 |
| D17 | The kit is docs only: no scripts, tool adapters, generated tool files or verification harness. The environment it is hydrated into handles all tooling, guided by principles in `SETUP.md` section 4 (owner direction). Supersedes D2, D3, D9, D10, D12, D13, D14. | DECIDED | 2026-11-02 |
