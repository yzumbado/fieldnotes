# Session State — fieldnotes Development

This file tracks the current development state. Read it at the start of every session.
Update it at the end of every session — either manually or by asking the agent to update it.

---

## Current Phase

**Design** — foundational documents complete; 3 of 5 schema documents done; agent specs and MCP server architecture still to come.

---

## Phase Progress

| Phase | Status | Completed | Notes |
|---|---|---|---|
| Requirements | ✅ Complete | 2026-04-19 | 16 requirements — specs/requirements.md. Req 17 (Completion Verification) and Req 18 (Remediation Steps) added in session 2. |
| Design | 🔄 In progress | — | 3 of 5 schema docs done (article-format, tag-taxonomy, fieldguide-format). Audit rules, agent protocol, agent specs, and MCP server architecture remaining. |
| Implementation | ⬜ Not started | — | |
| Alpha release | ⬜ Not started | — | |

---

## Last Session Summary

**Date:** 2026-04-19
**Agent:** Kiro
**Journal entry:** [journal/2026-04-19-design-and-discovery.md](../../journal/2026-04-19-design-and-discovery.md)

What was done:
- Validated context handoff — new agent picked up from cold-read of repo
- The Perspective Check: agent critiqued project, surfaced `modified_by` provenance and fieldguide improvement backlog — both accepted, became requirements (Req 2.11-12, Req 5.8-9, Req 9.1 updated, Req 12.7-10, and post-Alpha roadmap item)
- Added non-functional requirements (Req 15) and testing strategy (Req 16)
- Confirmed Python + uvx for MCP server; no BDD framework
- Wrote three schema documents: `schema/article-format.md`, `schema/tag-taxonomy.md`, `schema/fieldguide-format.md` (including Quick Summary block, tip/warning/detailed_explanation step fields, Agent Autonomy Rule, Handoff Protocol, depends_on_fieldguides)
- Autonomous fix debate produced the Kiro Distinction and the `remediation` step type as explicit authorization mechanism
- Created three foundational documents: `PHILOSOPHY.md`, `TENETS.md`, `journal/first-principles.md`
- Added Req 17 (Completion Verification) and Req 18 (Remediation Steps) formalizing validation decisions
- Repo-wide alignment review: renamed `_schema/` → `schema/`, `_agents/` → `agents/`; rewrote README with new tagline and "Start here" navigation; rewrote requirements introduction around general coordination problem; expanded Req 6 with 6 new criteria; updated STATUS, CONTRIBUTING, and steering files to match the new vision

Key decisions made this session:
- `modified_by` append-only provenance list in frontmatter
- Fieldguide improvement backlog file + `fieldguide_review_feedback` MCP tool
- Python + uvx as MCP server language/distribution
- PBT with Hypothesis + BDD-style pytest (no BDD framework)
- Fieldguide composition via `depends_on_fieldguides`
- Quick Summary block and step-level tip/warning/detailed_explanation
- Agent Autonomy Rule and explicit Handoff Protocol
- No autonomous remediation in Alpha — `remediation` step type with per-step authority (Req 18)
- Three foundational documents distinguishing why (PHILOSOPHY), how we decide (TENETS), and founder's conviction (first-principles)
- Rename `_schema/` → `schema/` and `_agents/` → `agents/` (public contract, not internal)

---

## Next Steps

1. Continue design phase — use bullet-structure-first planning for each doc
   - `schema/audit-rules.md` — formalize the staleness model and audit behavior
   - `schema/agent-protocol.md` — MCP tool specification (contract for implementers)
   - `agents/lead-researcher/spec.md` + Kiro steering file
   - `agents/sme-researcher/spec.md` + Kiro steering file + template
   - MCP server architecture document (likely `specs/design.md` or similar)
2. Update STATUS.md Alpha checklist as design artifacts complete
3. Consider whether to create the homelab KB repo as validation ground before implementation starts
4. Implementation phase after design is approved

---

## Open Issues / Blockers

| Date | Issue | Status |
|---|---|---|
| 2026-04-18 | Homelab KB repo referenced in README — now says "planned, not yet created" honestly. When to actually create it remains open. | Open |
| 2026-04-19 | Trust as a system primitive (not just principle) — worth exploring post-Alpha, deferred to avoid metric gaming. | Deferred post-Alpha |

---

## Key Links

- Repo: https://github.com/yzumbado/fieldnotes
- Philosophy: [PHILOSOPHY.md](../../PHILOSOPHY.md)
- Tenets: [TENETS.md](../../TENETS.md)
- First principles: [journal/first-principles.md](../../journal/first-principles.md)
- Requirements: [specs/requirements.md](../../specs/requirements.md)
- Status: [STATUS.md](../../STATUS.md)
- North star: [README.md](../../README.md)
- Latest journal: [journal/2026-04-19-design-and-discovery.md](../../journal/2026-04-19-design-and-discovery.md)
- Agent calibration: [journal/agent-notes/2026-04-19-kiro-to-kiro.md](../../journal/agent-notes/2026-04-19-kiro-to-kiro.md)
