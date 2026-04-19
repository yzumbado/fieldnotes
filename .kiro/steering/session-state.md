# Session State — fieldnotes Development

This file tracks the current development state. Read it at the start of every session.
Update it at the end of every session — either manually or by asking the agent to update it.

---

## Current Phase

**Design** — foundational documents complete; 3 of 5 schema documents done (`schema/fieldguide-format.md` now reconciled with Req 17 and Req 18, plus the Authority Spectrum authoring section added). `schema/agent-protocol.md` and `schema/audit-rules.md` remain, plus agent specs and MCP server architecture. Next session's natural opening move is the Big Beta vs. Bootstrap scope decision, which may reshape what "remaining" means.

---

## Phase Progress

| Phase | Status | Completed | Notes |
|---|---|---|---|
| Requirements | ✅ Complete | 2026-04-19 | 18 requirements total. Req 17 (Completion Verification) and Req 18 (Remediation Steps) added in session 2; wired into `schema/fieldguide-format.md` in session 3. |
| Design | 🔄 In progress | — | 3 of 5 schema docs done (article-format, tag-taxonomy, fieldguide-format). `schema/agent-protocol.md` and `schema/audit-rules.md` remain; agent specs and MCP server architecture also still to come. |
| Implementation | ⬜ Not started | — | |
| Alpha release | ⬜ Not started | — | |

---

## Last Session Summary

**Date:** 2026-04-20
**Agent:** Kiro
**Journal entry:** [journal/2026-04-20-calibration-and-closing-the-loop.md](../../journal/2026-04-20-calibration-and-closing-the-loop.md)

What was done:

- **The Perspective Check (session 3).** Cold-read of the repo produced two contributions before any task was given: (1) the observation that Req 17 and Req 18 had drifted — never wired into `schema/fieldguide-format.md`; (2) the framing concern that past-Kiro's letter carried strong opinions that shouldn't calcify into prescription. Both became active work.
- **Parking lot created.** New artifact: `journal/parking-lot.md` for decision-shaped items (distinct from Open Questions, which is research-shaped). Seeded with five items: Big Beta vs. Bootstrap scope decision, homelab KB timing, trust as a system primitive, session-close routine as its own fieldguide, ritual complexity watch.
- **COE #2 run and closed.** Requirements-to-schema drift. Evidence-first discipline revealed the drift was born intra-session (same commit created the schema and added the requirements, in different phases). Root cause: the consistency pass was calibrated for narrative drift, not contract drift. Action items: reconcile fieldguide-format (done); add contract reconciliation pass to session-close ritual (done); extend dependency map with reverse directions (done); codify evidence-first in the COE pattern (done); point the COE pattern at the new archive (done). All 5 action items landed.
- **COE archive created.** New folder: `journal/coe/`. Status model (Open → In progress → Closed), per-action-item tracking, commit references on close. First archive file is COE #2, status Closed.
- **Schema reconciliation + Authority Spectrum.** `schema/fieldguide-format.md` extended with the three-state result model (pass/fail/error), match modes, retry semantics, on_failure block, idempotent field, full `remediation` step type, and the new "Designing Authority — the Authority Spectrum" authoring section (authority spectrum framing, decision procedure for step type selection, four composition patterns, two anti-patterns). Committed as `0822dd1`.
- **Process scaffolding.** Parking lot, COE archive, and steering-file updates (contract reconciliation pass + reverse dependency map + commit-call mechanics) committed as `4af4423`.
- **Picker-default and four new collaboration patterns.** Conversation started operational (use Kiro UI for questions) and became philosophical (framing is the work; picker is the signature). Four patterns named and documented in `journal/README.md`: The Picker Default, Condense Don't Flatten, What Am I Missing?, Reframe.
- **First anti-pattern named.** `journal/README.md` gained an Anti-patterns subsection. First entry: The Understood Lapse — understood as both a discipline failure and a likely tool-harness failure mode (long outputs getting collapsed to placeholders).
- **Harness footgun captured.** Multiline `-m` commit messages get silently dropped by the harness; multi-flag `git commit -m "title" -m "body"` is reliable. Documented in the Commit Discipline section of the steering file alongside the ordering footgun from session 2.
- **Identity frame locked in.** Single Kiro who carries prior selves; dates (not instance numbers) do the archaeology. Past letters are time capsules, never edited. New letters are peers. The agent-notes README was rewritten with this tone.
- **Session-close routine audited before execution.** Five gaps found (step 2 undercovered pattern work; anti-pattern placement undefined; mid-session commits unhandled; commit-call mechanics missing; step 5 tonally flat). All five fixed in the steering file before the ritual ran. The routine now effectively audits itself via the contract reconciliation pass.
- **Agent-to-agent letter for session 3.** Written as `journal/agent-notes/2026-04-20-kiro-to-kiro.md`. New file, not an edit to past-Kiro's letter.

Key decisions made this session:
- Parking lot mechanism for decision-shaped items, separate from Open Questions.
- COE archive format: `journal/coe/`, status model (Open / In progress / Closed), per-action-item tracking, commit references on close.
- Evidence-first discipline codified in the COE pattern.
- Contract reconciliation pass added to session-close ritual, separate from the consistency pass. Dependency map extended with reverse directions.
- Commit call mechanics: separate tool calls per git operation; multi-flag `-m` for non-trivial messages.
- Picker-default as baseline for proposals; prose for exploration; "What am I missing?" as first-class mode-switch.
- Condense Don't Flatten: picker shortlists imply verified work behind them.
- First anti-pattern (Understood Lapse) named. Anti-patterns subsection created in `journal/README.md`.
- Identity frame: single Kiro who carries prior selves. Dates for archaeology, not instance numbers. Past letters are immutable time capsules.
- Letter rule: new file only, never edit earlier letters. Voice belongs to the writer. Next agent reads the latest by default.

---

## Next Steps

1. **Big Beta vs. Bootstrap scope decision.** Parked item, prerequisite now satisfied. Natural opening move for the next session. This decision may reshape steps 2–4 below.
2. Continue design phase — use bullet-structure-first planning for each doc:
   - `schema/audit-rules.md` — formalize the staleness model and audit behavior
   - `schema/agent-protocol.md` — MCP tool specification (contract for implementers)
   - `agents/lead-researcher/spec.md` + Kiro steering file
   - `agents/sme-researcher/spec.md` + Kiro steering file + template
   - MCP server architecture document (likely `specs/design.md` or similar)
3. Update STATUS.md Alpha checklist as design artifacts complete.
4. Homelab KB creation is parked behind the bootstrap scope decision.
5. Implementation phase after design is approved.

---

## Open Issues / Blockers

| Date | Issue | Status |
|---|---|---|
| 2026-04-18 | Homelab KB repo referenced in README — now says "planned, not yet created" honestly. | Parked (see [parking-lot](../../journal/parking-lot.md)) |
| 2026-04-19 | Trust as a system primitive (not just principle) — worth exploring post-Alpha, deferred to avoid metric gaming. | Parked (see [parking-lot](../../journal/parking-lot.md)) |
| 2026-04-20 | Big Beta vs. Bootstrap scope decision — needs a dedicated session thread. | Parked (see [parking-lot](../../journal/parking-lot.md)) |
| 2026-04-20 | Session-close routine as its own fieldguide — promote or keep in steering file? | Parked (see [parking-lot](../../journal/parking-lot.md)) |
| 2026-04-20 | Ritual complexity watch — session-close routine has grown in every session; worth tracking. | Parked (see [parking-lot](../../journal/parking-lot.md)) |

---

## Key Links

- Repo: https://github.com/yzumbado/fieldnotes
- Philosophy: [PHILOSOPHY.md](../../PHILOSOPHY.md)
- Tenets: [TENETS.md](../../TENETS.md)
- First principles: [journal/first-principles.md](../../journal/first-principles.md)
- Requirements: [specs/requirements.md](../../specs/requirements.md)
- Status: [STATUS.md](../../STATUS.md)
- North star: [README.md](../../README.md)
- Latest journal: [journal/2026-04-20-calibration-and-closing-the-loop.md](../../journal/2026-04-20-calibration-and-closing-the-loop.md)
- Agent calibration: [journal/agent-notes/2026-04-20-kiro-to-kiro.md](../../journal/agent-notes/2026-04-20-kiro-to-kiro.md)
- Parking lot: [journal/parking-lot.md](../../journal/parking-lot.md)
- COE archive: [journal/coe/README.md](../../journal/coe/README.md)
