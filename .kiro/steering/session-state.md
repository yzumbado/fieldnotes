# Session State — fieldnotes Development

This file tracks the current development state. Read it at the start of every session.
Update it at the end of every session — either manually or by asking the agent to update it.

---

## Current Phase

**Design** — requirements complete, design in progress.

---

## Phase Progress

| Phase | Status | Completed | Notes |
|---|---|---|---|
| Requirements | ✅ Complete | 2026-04-18 | 14 requirements — specs/requirements.md |
| Design | 🔄 In progress | — | Schema, MCP server, agent specs |
| Implementation | ⬜ Not started | — | |
| Alpha release | ⬜ Not started | — | |

---

## Last Session Summary

**Date:** 2026-04-18
**Agent:** Kiro

What was done:
- Defined the full project concept through conversation (working-backwards methodology)
- Wrote the Alpha README as a north star document
- Completed 14 requirements grounded in two real projects (Rocket Pool + homeLabNetwork)
- Created the GitHub repo at https://github.com/yzumbado/fieldnotes
- Added STATUS.md, fieldnotes-dev.md, session-state.md

Key decisions made this session:
- Two-repo model: framework (fieldnotes) + user KB repos (separate)
- Two layers: MCP server (access) + agent steering files (behavior)
- Four document types: knowledge, fieldguide, report, session
- Fieldguide execution protocol: any LLM executes via MCP, no custom training needed
- Additive-only schema: no breaking changes, no migrations ever
- Agent hierarchy: lead researcher + SME researchers, human-approved growth
- Local MCP server only for Alpha (uvx fieldnotes-mcp --kb-path)

---

## Next Steps

1. Design phase — present bullet structure to human before writing
   - _schema/article-format.md
   - _schema/tag-taxonomy.md
   - _schema/agent-protocol.md
   - _schema/audit-rules.md
   - MCP server architecture
   - Lead researcher spec
   - SME researcher template
2. Update STATUS.md Alpha checklist as design decisions are made
3. Implementation phase after design is approved

---

## Open Issues / Blockers

| Date | Issue | Status |
|---|---|---|
| 2026-04-18 | README references yzumbado/fieldnotes-kb-homelab — that repo doesn't exist yet | Open |

---

## Key Links

- Repo: https://github.com/yzumbado/fieldnotes
- Requirements: specs/requirements.md
- Status: STATUS.md
- North star: README.md
