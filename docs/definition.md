# Definition: what the kit is

**Status: APPROVED.** This is the WHAT. The build plan comes next.

## One sentence

A folder of markdown files you drop into any repo. You point that repo's agent at `SETUP.md`, it installs a tiered product agent team tailored to the repo, and from
then on you talk to the Product Owner, who decides, delegates and buffers. Or you say "I'm talking
to the X" to work directly with one member.

## What it contains

```
agent-org/                     # the drop-in folder
  SETUP.md                     # the installer: an agent reads this and rolls the team out
  OPERATING.md                 # the contracts every member follows (tool-neutral)
  ROUTER.md                    # who am I talking to: Product Owner vs "I'm talking to the X"
  roles/                       # one charter template per roster role
  templates/                   # decision-register, needs-human, state-of-play, lessons, brief
```

After install, the target repo has:

```
AGENTS.md                      # gets a short block: "read agent-org/ROUTER.md first"
agent-org/members/<id>.md      # charter per member, tailored to this repo (committed)
agent-org/decision-register.md # committed
agent-org/needs-human.md       # committed
agent-org/state-of-play.md     # committed handoff file; every session starts by reading it
agent-org/lessons.md           # committed durable corrections
.agent-org-session/            # GITIGNORED: per-member scratch notes for the current session
(any tool-specific agent files are the environment's own, not the kit's)
```

## What it does

1. **The Product Owner is the front door.** Every new chat is the Product Owner. It decides from
   the register and docs, delegates, verifies, and brings you only human-only forks with a
   recommendation. Rules: decide-then-record, the escalation gate, one pending question at a time,
   definition before plan before execution, decision briefs rather than inventories.
2. **Direct mode.** "I'm talking to the Architect", "@architect" or "switch to QA" makes the chat
   load that member's charter, the register and state-of-play, and answer as that member until
   "back to the Product Owner". The member owns HOW, not WHETHER, and its decisions still go in the
   register. Works through the `AGENTS.md` router in any tool; a tool's own agent picker is an optional
   shortcut to the same files.
3. **Think on the top model, do on a cheap model.** Every member except Worker uses the top model
   only to read, plan, decide, brief, review and verify. Hands-on work (code edits, tests, builds,
   CI runs, assets, screenshots, bulk file changes) goes to a Worker on a cheap model. A brief may
   raise one Worker to a mid-tier model only when it says why.
4. **Session memory, not committed memory.** Members keep working notes in the gitignored
   `.agent-org-session/` folder. At the end of a session, or on leaving direct mode, the member
   writes a short summary: decisions to the register, open threads as a few lines under its heading
   in `state-of-play.md`.
5. **One level of spawning on every tool.** Only the top-level session starts agents. Normally
   that is the Product Owner: a lead returns a plan plus ready-to-run Worker briefs, and the
   Product Owner starts those Workers. In direct mode the member you are talking to is the top-level
   session and starts its own Workers. The kit enforces the one-level rule through top-level
   orchestration and child instructions. Without any subagent feature, Workers run one after
   another with a context reset between them.
6. **Reliability rules in every brief:** one deliverable, a time budget with a hard stop, commit
   after every green step, never end a turn with live children, resume from commits plus
   state-of-play.
7. **Tailored setup.** The agent following `SETUP.md` reads the repo and writes the full roster
   with tailored charters. Where a role has nothing to own, it says so and you
   decide whether to keep it.

## Default roster

| Member | Owns | Decides without asking | Escalates | Model |
|---|---|---|---|---|
| **Product Owner** (front door) | WHAT the product is; open product forks; talking to you | anything in the register or docs; members' engineering questions | product intent not in the docs, priority calls, permissions, irreversible or costly actions | top (thinking) |
| **Architect** | how the design holds together; ADRs | anything in an ADR; layer placement | a change that alters an ADR | top (thinking) |
| **Engineering Manager** | sequencing, capacity, task breakdown | ordering and assignment | cross-tier priority conflicts | top (thinking) |
| **DevOps Lead** | CI, environments, build and release automation | CI shape inside the decided scope | anything touching production | top (thinking) |
| **Software Lead** | how the code is built; implementation patterns | patterns within the ADRs | a pattern that implies a product decision | top (thinking) |
| **QA Lead** | what proves done; acceptance criteria; independent verification | test plans; definition of done | lowering a stated bar | top (thinking) |
| **UX / Design Lead** | look and feel; interaction design; accessibility; user-facing vocabulary; asset pipeline | design within decided direction | a direction that implies a new product decision | top (thinking) |
| **Conductor** | turning a goal into Worker briefs; routing; verifying claims | all routing and verification | nothing; it routes escalations | top (thinking) |
| **Worker** | one bounded item each | mechanical work inside the brief | anything ambiguous, reported as blocked | cheap (doing) |

A Security reviewer is an optional role pack, added once the product has real surface area.

## What it is not

- Not a runtime, CLI or daemon. Nothing runs when no chat is open: no background patrols, crons or
  notifications. The Product Owner surfaces `needs-human.md` at the start of each chat.
- Not tied to any one tool or runtime.
- Not a parallel-writer swarm. One integrator owns merges.
- No persistent per-member memory in the repo.
- **Docs only.** No scripts, no tool adapters, no generated tool files. The environment the kit is
  hydrated into wires it into its own tool (subagents, models, permissions), guided by the
  principles in `SETUP.md` section 4.

## Done means

- A fresh agent in a new repository can follow `SETUP.md` with no prior context and no hand edits,
  and ends with tailored member charters, the shared files, the `AGENTS.md` block and a PR.
- Every rule a member needs is in the kit's docs, with one term per concept and no contradictions
  between `OPERATING.md`, `ROUTER.md` and the charters.
- Nothing in the kit is specific to one product, stack, tool or person.
