# fieldnotes — Implementation Status

> Last updated: 2026-04-19 | Agent: Kiro

## Current phase: Design

Requirements are complete. Design is in progress.
**Nothing is implemented yet.** The MCP server does not exist. Most agent steering files do not yet exist — only the project's foundational documents (PHILOSOPHY, TENETS, first-principles) and the project-wide collaboration guide.

Schema progress: 3 of 5 schema documents complete (article-format, tag-taxonomy, fieldguide-format). `schema/fieldguide-format.md` was reconciled with Req 17 and Req 18 in session 3, and the "Designing Authority — the Authority Spectrum" authoring section was added. Audit rules and agent protocol remain.

The README describes what Alpha will be — not what exists today. This file is the honest answer to "can I use this today?" (Not yet — but we're closer than we were yesterday.)

---

## Phase tracker

| Phase | Status | Notes |
|---|---|---|
| Requirements | ✅ Complete | 18 requirements — see [specs/requirements.md](specs/requirements.md) |
| Design | 🔄 In progress | 3 of 5 schema docs complete; MCP server architecture and agent specs pending |
| Implementation | ⬜ Not started | |
| Alpha release | ⬜ Not started | |

---

## What exists today

### Foundational documents
- `README.md` — working-backwards Alpha north star document
- `PHILOSOPHY.md` — what fieldnotes believes and why it looks the way it does
- `TENETS.md` — operational principles (9 tenets) that govern decisions
- `journal/first-principles.md` — founder's statement in first person
- `journal/agent-notes/2026-04-19-kiro-to-kiro.md` — first agent-to-agent calibration letter (session 2)
- `journal/agent-notes/2026-04-19-kiro-to-kiro-session-3.md` — session 3 calibration letter (picker-default, harness footguns, identity frame)

### Specs
- `specs/requirements.md` — 18 requirements with acceptance criteria (Req 17 Completion Verification, Req 18 Remediation Steps added in session 2)
- `specs/README.md` — explains the spec process and origin story

### Schema (design phase — in progress)
- `schema/article-format.md` — canonical article template + field definitions
- `schema/tag-taxonomy.md` — tag system + tagging guidelines for agents
- `schema/fieldguide-format.md` — fieldguide step schema, execution protocol (three-state pass/fail/error), Agent Autonomy Rule, Handoff Protocol, Quick Summary block, depends_on_fieldguides, tip/warning/detailed_explanation, completion block (match modes, retry, on_failure, idempotent), full `remediation` step type, and the Authority Spectrum authoring section (composition patterns, decision procedure, anti-patterns)

### Process scaffolding
- `.kiro/steering/fieldnotes-dev.md` — agent collaboration guide (now includes contract reconciliation pass, reverse dependency map, commit-call mechanics)
- `.kiro/steering/session-state.md` — session continuity state
- `journal/parking-lot.md` — decision-shaped items, append-only with pointers to resolution
- `journal/coe/` — Correction of Error archive with status model (Open / In progress / Closed); COE #2 (requirements-to-schema drift) is Closed
- `journal/README.md` — collaboration patterns and anti-patterns, Open Questions, index of journal entries and agent notes

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
- [ ] `examples/minimal-kb/` — one fieldguide with all five step types (`human_required`, `agent_executable`, `approval_gate`, `verification`, `remediation`)

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
| 2026-04-19 | Agent-to-agent letters pattern | New artifact type: first-person calibration letters from one agent instance to the next, stored in `journal/agent-notes/`. Separate from journal entries (human-facing narrative). Agents read the most recent letter at session start; optionally append one at session close when new patterns emerge. |
| 2026-04-19 | COE pattern for process failures | When a process failure ships (not a typo or small bug), apply 5 Whys to find root cause, write action items including process changes. Triggered explicitly when the builder senses a deeper issue. Formalized in journal collaboration patterns. |
| 2026-04-19 | Consistency pass at session close | Before committing session close, re-read STATUS.md, README.md, and the steering files end-to-end as a new reader would — not as the writer tracking diffs. Catches narrative drift that additive updates leave behind. |
| 2026-04-19 | Parking lot mechanism for decision-shaped items | `journal/parking-lot.md` — append-only with explicit revisit triggers. Distinct from Open Questions (research-shaped): parking lot holds concrete A-or-B decisions we defer on purpose. Resolved items stay in the file with pointers to where the decision landed. |
| 2026-04-19 | Contract reconciliation pass at session close | Added as step 6 of the session-close ritual, separate from the consistency pass. Walks the dependency map in both directions for every file modified in the session. Catches contract drift — requirements and schema disagreeing, design and schema disagreeing, intra-session drift where co-produced artifacts weren't reconciled. Dependency map in the steering file extended with reverse directions. Driven by COE #2 findings. |
| 2026-04-19 | COE archive format | New folder `journal/coe/` with status model (Open / In progress / Closed), per-action-item tracking, commit references on close. Pattern definition in `journal/README.md` extended with evidence-first discipline and pointer to the archive. |
| 2026-04-19 | Picker-default as baseline communication mode | Structured options via Kiro UI as the default shape for proposals and sign-off; prose for exploration, debate, and catch-up. Every non-trivial picker includes "What am I missing?" as a first-class mode-switch. Framing above the picker is the work; the picker is the signature. Four patterns named in `journal/README.md`: The Picker Default, Condense Don't Flatten, What Am I Missing?, Reframe. Provisional with reversal clause. |
| 2026-04-19 | First anti-pattern named: The Understood Lapse | Understood as both a discipline failure and a likely tool-harness failure mode (long intended outputs getting collapsed to placeholders). Correction is attention *and* diagnosis. New Anti-patterns subsection created in `journal/README.md`, cross-linked from the steering file's Do-Not list. |
| 2026-04-19 | Identity frame — single Kiro who carries prior selves | Not numbered instances. Dates (not instance numbers) do the archaeology when precision is needed. Past letters are immutable time capsules; new letters are peers, never revisions. Letter rule rewritten with openness tone — gift, not report. |
| 2026-04-19 | Verify session date with `date` before dating artifacts | Session 3 originally dated everything 2026-04-20 based on an unverified assumption that "next session = next calendar day." Real date was 2026-04-19 (same day as session 2, later in the day). Running `date` at session start is trivial; the cost of skipping it was a full rename + content-fix pass. See COE #3. |

---

## Post-Alpha roadmap

| Item | Description | Depends on |
|---|---|---|
| Automated feedback-to-edit pipeline | Lead researcher reads backlog, drafts fieldguide step changes, presents to human for approval, applies via `kb_update` | Alpha: backlog file, `fieldguide_review_feedback` tool, human triage workflow |
