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
| Requirements | ✅ Complete | 14 requirements — see [specs/requirements.md](specs/requirements.md) |
| Design | 🔄 In progress | Schema, MCP server, agent specs |
| Implementation | ⬜ Not started | |
| Alpha release | ⬜ Not started | |

---

## What exists today

- `README.md` — working-backwards Alpha north star document
- `specs/requirements.md` — 14 requirements with acceptance criteria
- `specs/README.md` — explains the spec process and origin story
- `.kiro/steering/fieldnotes-dev.md` — agent collaboration guide
- `.kiro/steering/session-state.md` — session continuity state
- Project structure scaffolded: `_schema/`, `mcp-server/`, `implementations/kiro/`, `examples/minimal-kb/`

---

## Alpha checklist

### Schema
- [ ] `_schema/article-format.md` — canonical article template + field definitions
- [ ] `_schema/tag-taxonomy.md` — shared tag system (domain, volatility, status)
- [ ] `_schema/agent-protocol.md` — generic MCP access layer spec
- [ ] `_schema/audit-rules.md` — staleness model + audit behavior
- [ ] `_schema/fieldguide-format.md` — fieldguide step schema + execution protocol

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
- [ ] `_agents/lead-researcher/spec.md`
- [ ] `_agents/lead-researcher/kiro/steering.md`
- [ ] `_agents/sme-researcher/spec.md`
- [ ] `_agents/sme-researcher/kiro/steering.md`
- [ ] `_agents/_template/` — copy-paste starting point for new SMEs

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

---

## Post-Alpha roadmap

| Item | Description | Depends on |
|---|---|---|
| Automated feedback-to-edit pipeline | Lead researcher reads backlog, drafts fieldguide step changes, presents to human for approval, applies via `kb_update` | Alpha: backlog file, `fieldguide_review_feedback` tool, human triage workflow |
