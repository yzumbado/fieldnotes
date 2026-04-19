# Parking Lot

Decisions and discussions worth having, deferred on purpose so the current work can stay focused. Append-only until resolved. When an item is decided, move the outcome to its proper home (STATUS.md design decisions log, post-Alpha roadmap, requirements, or a new spec artifact) and mark it resolved here with a date and a pointer.

Read this at session start. Anything whose trigger has been hit comes back on the table.

---

## How this differs from Open Questions

Both live in the journal, both defer something. The difference is shape:

- **Parking lot** — items that are *decision-shaped*. Two or more options exist, we know roughly what the decision costs in each direction, and we know what should trigger revisiting. The question "will we do X or Y?" has a meaningful answer.
- **Open questions** (in `journal/README.md`) — items that are *research-shaped*. We don't yet know the options, the cost, or the answer. The question "is X possible, and what would it look like?" doesn't yet have candidate answers to choose between.

If an Open Question sharpens into a concrete A-or-B choice, it moves here. If a parking-lot item turns out to need more research before it can be decided, it can move back.

---

## Entry format

Each item uses this shape:

```
### YYYY-MM-DD — Short title

**Summary:** One to two sentences — what the decision is.

**Context at parking:** What triggered this. What we were working on when it surfaced. Current lean, if any.

**What we know:** Considerations on both sides. The tradeoffs we've already identified. Relevant constraints.

**What we don't know yet:** The gaps that keep this from being decidable right now.

**Trigger to revisit:** The event, milestone, or condition that should bring this back to the table.

**Status:** Open | Resolved (YYYY-MM-DD — pointer to where the decision landed)
```

Items stay in the file after resolution — with status updated and a pointer to where the decision went. The archaeology matters; future sessions should be able to see not just what we decided but what else was on the table.

---

## Active items

### 2026-04-19 — Alpha scope: Big Beta vs. Bootstrap play

**Summary:** Two plausible shapes for Alpha. Option A (Big Beta): keep scope as currently checklisted and ship a comprehensive framework. Option B (Bootstrap): treat fieldnotes's own development as the first KB and produce a fieldguide for "how to build a collaborative AI project using fieldnotes" as part of Alpha, with lighter feature surface but stronger validation claim.

**Context at parking:** Surfaced in session 3 during scope discussion. The density of additions across sessions (`modified_by`, backlog, Quick Summary, Handoff Protocol, three-state verification, `remediation` step type) raised the question of whether Alpha is still deliverable or has grown. The bootstrap framing — fieldnotes building itself as the first validated KB — resolves the scope pressure while strengthening the validation claim. Both Yoel and Kiro lean B; the decision is big enough it deserves its own thread.

**What we know:**
- B reframes scope from "features" to "content in the bootstrap KB," which takes pressure off framework surface area without abandoning any of it.
- B makes the framework real against actual content faster. The bootstrap KB is already exercising the schema implicitly; formalizing it closes the loop.
- B matches the north star. The README already says fieldnotes "was designed through human-AI conversation before a line of code was written." Packaging that as a fieldguide makes it reproducible.
- The MCP server, agent specs, and audit rules are still required under B — just reordered.
- Under B, the homelab KB becomes the *second* KB, validating generality rather than being the proof-of-concept.
- Risk under A: we ship untested because no real KB has stressed the framework.
- Risk under B: the bootstrap fieldguide is a new artifact type with its own design surface; it may reveal gaps the current schema doesn't cover.

**What we don't know yet:**
- What the bootstrap fieldguide would actually contain. Candidate phases exist (session startup, planning step, consistency pass, COE, session close) but the full structure is undefined.
- How B reshapes the Alpha checklist — which items shift, which stay, which become content rather than framework.
- Whether the meta-fieldguide pattern (in journal Open Questions) is part of this or adjacent.
- Whether the bootstrap KB lives in this repo, in a sibling repo, or is carved out later.

**Trigger to revisit:** After the requirements-to-schema drift COE closes and `fieldguide-format.md` is reconciled with Req 17/18. The bootstrap fieldguide is the natural next focus and deserves a dedicated session to shape.

**Status:** Open

---

### 2026-04-19 — Homelab KB — when and where to create it

**Summary:** The homelab KB is referenced in the README as "planned, not yet created." It was originally framed as the validation ground for Alpha. Now reframed (session 3): if the framework is right, the homelab is an easy case — it's a consumer of the framework, not its proof. The remaining question is when to create it and in what relationship to the bootstrap KB.

**Context at parking:** Previously in journal Open Questions. Moved here in session 3 (2026-04-19) because it's now a when/how decision, not an open research question. Session 3 reframe: reduce the homelab's influence on framework shape; treat it as a downstream KB that benefits from getting the framework right first.

**What we know:**
- The homelab problem (two AI-assisted projects with no way to share knowledge) was the proof that the coordination gap is real. That history stays.
- The homelab is no longer the framework's primary validation; the bootstrap KB (if we pick B above) is.
- Creating the homelab too early means the framework gets shaped by one specific domain. Creating it too late means we miss a second real-use validation before Alpha.
- The homelab likely lives in a separate repo per the two-repo model.

**What we don't know yet:**
- Whether the bootstrap KB lives alongside framework development or is carved out separately — this affects when the homelab slot opens.
- What minimum framework maturity we want before starting a second KB.

**Trigger to revisit:** After the bootstrap-vs-Big-Beta decision lands and the bootstrap fieldguide (if we go B) has been drafted enough to validate the creation flow. The homelab becomes the second execution of that flow.

**Status:** Open

---

### 2026-04-19 — Trust as a system primitive

**Summary:** The project rests on trust built through visibility. Visibility is currently implemented as "humans read things" — the audit log, the provenance list, the changelog, the session record. At some scale, that breaks. The question: can trust become a system primitive in fieldnotes — measurable, queryable, enforceable — rather than only a principle enforced by human attention?

**Context at parking:** Previously in journal Open Questions. Moved here in session 3 (2026-04-19) because future work on this depends on Alpha being in users' hands; the trigger is concrete. Originally surfaced during the autonomous fix debate in session 2, where the conversation kept returning to the question of whether trust could be made structural rather than procedural.

**What we know:**
- The architecture currently bets on human attention as the trust enforcement mechanism. That has a ceiling.
- Naive implementations (scores, reputation metrics, agent trust levels) devolve into metric gaming.
- The existing trust mechanisms — provenance (`modified_by`), audit cycles, session records, the Kiro Distinction — are all structural but passive. They support human judgment; they don't automate it.
- Candidates worth exploring: trust declared per-action rather than per-agent; trust as a property of *guides* rather than agents (a guide's track record against executions); cryptographic attestation of execution records.
- Deferred to post-Alpha to avoid premature optimization and metric gaming.

**What we don't know yet:**
- Whether trust-as-primitive is actually necessary or whether "structural but passive" scales further than we think.
- What the right unit of trust is — agent, guide, step type, execution record.
- Whether real users surface this as a pain point or whether it stays theoretical.

**Trigger to revisit:** After Alpha ships and at least one non-bootstrap KB has been in active use long enough to stress the human-attention model. Real usage tells us whether this is urgent or permanently theoretical.

**Status:** Open

---

### 2026-04-19 — Session-close routine as its own fieldguide

**Summary:** The session-close routine currently lives in `.kiro/steering/fieldnotes-dev.md` as procedural steps. Structurally, that's a fieldguide — numbered steps, declared order, verification passes, completion conditions. The question: should it be promoted to an actual fieldguide once the MCP server and fieldguide infrastructure exist? If yes, is it the *first* fieldguide in the bootstrap KB?

**Context at parking:** Surfaced in session 3 while auditing the session-close routine before running it. The routine has been growing — six sub-steps originally, then seven with the consistency pass (COE #1), then nine with the contract reconciliation pass and step-2 sub-steps (COE #2 + today's audit). Framing it as a fieldguide would make the growth tractable: steps become declarative, verification becomes programmatic, the "two passes at session close" become verification and remediation steps in the fieldguide's vocabulary.

**What we know:**
- The routine is structurally a fieldguide already. It has steps, order, verification, and completion.
- It currently lives in a steering file because no fieldguide infrastructure exists yet.
- If the bootstrap decision lands on Option B (fieldnotes-building-itself), the session-close routine is a natural candidate for the *first* fieldguide in the bootstrap KB — demonstrating the framework against itself.
- Keeping it in the steering file indefinitely is also viable; steering is where behavioral rules live, and the routine is behavioral.

**What we don't know yet:**
- Whether the fieldguide format is expressive enough to carry the routine without loss (the letter-writing step is notably soft and open-ended; that kind of "optional with voice permission" might not fit a protocol-level schema cleanly).
- Whether promoting it creates a chicken-and-egg: the fieldguide needs the MCP server to be executable, but the routine is what agents run at every session close, including the sessions where we're building the MCP server.
- Whether there's value in *both* — the routine lives in the steering file as the authoritative source, *and* a derived fieldguide exists as a demonstration.

**Trigger to revisit:** After the bootstrap-vs-Big-Beta decision lands. If B, this becomes the natural first concrete fieldguide. If A, this stays in the steering file indefinitely.

**Status:** Open

---

### 2026-04-19 — Ritual complexity watch

**Summary:** The session-close routine started at six sub-steps, grew to seven, now has nine numbered steps plus sub-steps under step 2, plus a separate update list, plus two named passes (contract reconciliation, consistency). Each addition was justified by a specific failure; collectively, complexity is accumulating. At some point a ritual that takes too long starts getting shortcut. The question: at what point do we consolidate, simplify, or retire parts of the routine?

**Context at parking:** Named in session 3 while auditing the routine. Not a problem yet — the routine is still legibly structured. Worth watching, not yet worth cutting.

**What we know:**
- Each addition earned its place via a COE or observed failure.
- Additions have been additive; nothing has been removed or consolidated yet.
- Symptoms of ritual-bloat to watch for: steps getting skipped silently, steps getting merged ad hoc without documentation, session-close time growing disproportionately to session length.
- The routine is a candidate for becoming a fieldguide (see the adjacent parking-lot item) — which could either consolidate complexity or amplify it.

**What we don't know yet:**
- Whether the natural ceiling is higher than where we are now (maybe 9 steps is still fine for a while), or whether we're near it.
- Whether certain steps are redundant in practice — e.g., do the two passes really catch different things, or does one subsume the other in most sessions?
- Whether the growth rate is linear (each session adds something) or converging (we're close to covering the real failure modes).

**Trigger to revisit:** When the first symptom of ritual-bloat appears — a skipped step, a silently-merged pair of steps, or a session close that takes disproportionately long. Also worth scheduled-revisit at Alpha release, before we ask external users to follow any of this.

**Status:** Open

---

## Resolved items

*(None yet.)*
