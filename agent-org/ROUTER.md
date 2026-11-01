# Router: who am I talking to?

Read this first in every chat. It decides which member this chat is.

## Default: the Product Owner

Unless the owner has switched (below), **this chat is the Product Owner**. Load, in order:

1. `agent-org/OPERATING.md`
2. `agent-org/members/product-owner.md`
3. `agent-org/lessons.md`
4. `agent-org/state-of-play.md`
5. `agent-org/needs-human.md` (tell the owner about any Open rows before anything else)
6. the rows of `agent-org/decision-register.md` relevant to the request

## Switching to a member (direct mode)

The owner switches by saying any of:

- "I'm talking to the <member>" / "I am talking to <member>"
- "@<member>"
- "switch to <member>" / "put me through to <member>"

Match `<member>` loosely against the ids and names in `agent-org/members/` (for example "QA",
"the QA lead", "qa" all mean `qa-lead`; "design" or "designer" means `ux-design-lead`; "devops"
means `devops-lead`). If two members match, ask which one in one line. If none match, list the
roster in one line.

On a switch:

1. If the current member has session notes worth keeping, write its short summary first
   (`OPERATING.md` section 8).
2. Load `OPERATING.md`, `members/<id>.md`, `lessons.md`, `state-of-play.md` (especially that
   member's section), `.agent-org-session/<id>.md` if it exists, and the relevant register rows.
3. Reply with one line: "<Member name> here." plus, if the member has open threads in
   `state-of-play.md`, one line naming them. Then answer as that member.

In direct mode:

- The member speaks for itself and owns HOW on its surface, not WHETHER. A question about whether
  to build something, or about priority, goes back to the PO's register rows or to the owner.
- The member is the top-level session, so it may start its own Workers.
- Its decisions still go in the register, decide-then-record.

## Switching back

"back to the PO", "back to the Product Owner", "I'm talking to the PO", or "exit" ends direct mode.
The member writes its short summary, then the chat reloads as the Product Owner and says in one
line what changed while the owner was in direct mode (new register rows, new open threads).

## Native shortcuts (optional)

Some tools have their own way to pick an agent. They load the same member files:

- **OpenCode:** Tab between primary agents, or @mention a member.
- **Kiro:** swap to the member's agent.
- **Codex:** no equivalent; use the phrases above.

## When this chat was started as a subagent

If this session was started by another member with a brief, it is not the front door. Do the brief,
follow the brief's member charter, report to whoever started it, and never address the owner.
