# fieldnotes — Implementation Status

> Last updated: 2026-04-18 | Agent: Kiro

## Current phase: Design

Requirements are complete. Design is in progress.
**Nothing is implemented yet.** The MCP server does not exist. The schema files do not exist. The agent steering files do not exist.

The README describes what Alpha will be — not what exists today. This file is the honest answer to "can I use this today?" (Not yet.)

---

## Phase tracker

| Phase | Status | Notes |
|---|---|---|
| Requirements | ✅ Complete | 16 requirements — see [specs/requirements.md](specs/requirements.md) |
| Design | 🔄 In progress | Schema, MCP server, agent specs |
| Implementation | ⬜ Not started | |
| Alpha release | ⬜ Not started | |

---

## What exists today

### Foundational documents
- `README.md` — working-backwards Alpha north star document
- `PHILOSOPHY.md` — what fieldnotes believes and why it looks the way it does
- `TENETS.md` — operational principles (9 tenets) that govern decisions
- `journal/first-principles.md` — founder's statement in first person

### Specs
- `specs/requirements.md` — 16 requirements with acceptance criteria
- `specs/README.md` — explains the spec process and origin story

### Schema (design phase — in progress)
- `schema/article-format.md` — canonical article template + field definitions
- `schema/tag-taxonomy.md` — tag system + tagging guidelines for agents
- `schema/fieldguide-format.md` — fieldguide step schema, execution protocol, Agent Autonomy Rule, Handoff Protocol, Quick Summary block, depends_on_fieldguides, tip/warning/detailed_explanation

### Steering and continuity
- `.kiro/steering/fieldnotes-dev.md` — agent collaboration guide
- `.kiro/steering/session-state.md` — session continuity state

### Scaffolded but empty
- `schema/` (audit-rules.md and agent-protocol.md pending)
- `agents/` (lead-researcher, sme-researcher, _template — all pending)
- `mcp-server/` (implementation phase)
- `implementations/kiro/` (kiro implementation guide pending)
- `examples/minimal-kb/` (example KB pending)

---

## Alpha checklist

### Schema
- [x] `schema/article-format.md` — canonical article template + field definitions
- [x] `schema/tag-taxonomy.md` — shared tag system (domain, volatility, status)
- [ ] `schema/agent-protocol.md` — generic MCP access layer spec
- [ ] `schema/audit-rules.md` — staleness model + audit behavior
- [x] `schema/fieldguide-format.md` — fieldguide step schema + execution protocol

### MCP Server
- [ ] `mcp-server/` — Python package, uvx-installable
- [ ] `kb_search` tool
- [ ] `kb_get` tool
- [ ] `kb_create` tool (with schema validation)
- [ ] `kb_update` tool
- [ ] `kb_audit` tool
- [ ] `fieldguide_load` tool
- [ ] `fieldguide_get_context` tool
- [ ] `fieldguide_advance` tool
- [ ] `fieldguide_submit_feedback` tool
- [ ] `fieldguide_review_feedback` tool
- [ ] `agent_propose` tool

### Agent Specs (Kiro implementation)
- [ ] `agents/lead-researcher/spec.md`
- [ ] `agents/lead-researcher/kiro/steering.md`
- [ ] `agents/sme-researcher/spec.md`
- [ ] `agents/sme-researcher/kiro/steering.md`
- [ ] `agents/_template/` — copy-paste starting point for new SMEs

### Example KB
- [ ] `examples/minimal-kb/fieldnotes.yml`
- [ ] `examples/minimal-kb/` — one knowledge article
- [ ] `examples/minimal-kb/` — one fieldguide with all four step types

### Kiro Implementation Guide
- [ ] `implementations/kiro/README.md`

---

## Design decisions log

| Date | Decision | Rationale |
|---|---|---|
| 2026-04-18 | Two-repo model | Framework and content must be separable — content is user-owned |
| 2026-04-18 | Additive-only schema | No breaking changes, no migrations, ever |
| 2026-04-18 | Local MCP server only for Alpha | Simplest path to validate the protocol |
| 2026-04-18 | Four document types | knowledge, fieldguide, report, session — each has a distinct lifecycle |
| 2026-04-18 | Fieldguide execution protocol | Any LLM with MCP access can execute a guide — no custom training needed |
| 2026-04-18 | Agent hierarchy with human-approved growth | Lead proposes SMEs, human approves — controlled expansion |
| 2026-04-18 | `modified_by` provenance history | Append-only list in frontmatter — queryable agent history without parsing changelogs |
| 2026-04-18 | Fieldguide improvement backlog | Feedback → structured backlog → human triage in Alpha; agent-proposed edits post-Alpha |
| 2026-04-18 | Python + uvx for MCP server | Best distribution story (uvx), Hypothesis for PBT, ruamel.yaml for round-trip YAML, already on target machines |
| 2026-04-18 | Testing: PBT + BDD-style pytest | Hypothesis for round-trip properties, Given/When/Then pytest structure, end-to-end tool tests, no BDD framework overhead |
| 2026-04-18 | Foundational documents separated | PHILOSOPHY.md (why), TENETS.md (how we decide), journal/first-principles.md (founder's statement) — different genres, different audiences |
| 2026-04-18 | Rename `_schema/` → `schema/`, `_agents/` → `agents/` | These are the public contract, not internal — the underscore misrepresented them |
| 2026-04-18 | No autonomous remediation in Alpha | Verification failures default to "stop and report." Remediation is declared by guide authors via an explicit step type with per-step authority (Req 18). Autonomous improvisation is out of scope — the Kiro Distinction. |
| 2026-04-18 | Fieldguide composition via `depends_on_fieldguides` | Splits large workflows into reusable guides. `fieldguide_load` enforces dependency completion. |
| 2026-04-18 | Quick Summary block + tip/warning/detailed_explanation | Fixed-format scan block at the top of every fieldguide; optional per-step fields for risks, shortcuts, and deferred context |
| 2026-04-18 | Agent Autonomy Rule + Handoff Protocol | Explicit rules: agents never ask humans to do what agents can do; transitions between step types follow declared handoff semantics |

---

## Post-Alpha roadmap

| Item | Description | Depends on |
|---|---|---|
| Automated feedback-to-edit pipeline | Lead researcher reads backlog, drafts fieldguide step changes, presents to human for approval, applies via `kb_update` | Alpha: backlog file, `fieldguide_review_feedback` tool, human triage workflow |
