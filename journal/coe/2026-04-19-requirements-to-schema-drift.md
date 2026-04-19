# COE — Requirements-to-Schema Drift

**Opened:** 2026-04-19 (session 3)
**Status:** Closed
**Last status change:** 2026-04-19

## The failure

`schema/fieldguide-format.md` shipped on 2026-04-19 (session 2's close) without reflecting Requirements 17 (Completion Verification) and 18 (Remediation Steps), even though those requirements were added to `specs/requirements.md` in the same commit. The schema document described the two-state PASS/FAIL completion model and had no mention of the `remediation` step type — both of which had been superseded by the new requirements landing in the same session.

Noticed by: Kiro (session 3, during pre-work review of the repo state — later same day, 2026-04-19).

## Evidence

Git archaeology, gathered before forming a hypothesis about cause.

### Commit history for the two files

```
git log --oneline --all -- specs/requirements.md schema/fieldguide-format.md

71a6cea 2026-04-19 docs: session close 2026-04-19 — design phase + philosophy discovery
87ec4f4 2026-04-18 spec: add non-functional requirements and testing strategy (Req 15-16)
8f5dd4c 2026-04-18 spec: add modified_by provenance and fieldguide improvement backlog
f93260a 2026-04-18 feat: initial fieldnotes framework — Alpha north star + requirements
```

Only four commits touch either file. The drift has to have been introduced in one of them.

### The suspect commit

Commit `71a6cea` (2026-04-19, session-close commit for session 2) is the only commit that creates `schema/fieldguide-format.md`. It is also the commit that introduces Req 17 and Req 18 into `specs/requirements.md`.

Stat for that commit:
```
schema/fieldguide-format.md | 720 ++++++++++++++++++++++  (created)
specs/requirements.md       |  81 ++++++++++++              (81 lines added)
```

The schema doc was born and the new requirements were added in the same atomic commit. There is no earlier version of `schema/fieldguide-format.md` that was later updated; the file was written from scratch in this one commit.

### Commit message, relevant excerpts

The commit message describes the session in phases:

> "Started as design phase continuation, ended with the project discovering its own philosophy."
>
> "Schema documents (new, 3 of 5 done): … schema/fieldguide-format.md — execution protocol, Quick Summary block, tip/warning/detailed_explanation, depends_on_fieldguides, Agent Autonomy Rule, Handoff Protocol"
>
> "Requirements expansion: … Req 17 (new): Completion verification (match modes, retry, three-state results, diagnostic execution, idempotent). Req 18 (new): Remediation steps (declared per-step authority, no autonomous fixes in Alpha)"

Req 17 and Req 18 are listed as products of the same session, but separately from the schema doc's bullet. The schema doc's bullet does not mention them, consistent with the schema having been written before they existed.

Corroborating narrative: `journal/2026-04-19-design-and-discovery.md` describes the session in three phases:
1. Design phase proper (article-format, tag-taxonomy, fieldguide-format written)
2. Autonomous fix debate (produces the `remediation` concept and the Kiro Distinction)
3. Philosophy spill + repo-wide alignment review (produces PHILOSOPHY/TENETS/first-principles, then reconciles README/STATUS/CONTRIBUTING/steering with the new vision)

Req 17 and Req 18 were added during phase 3 (the alignment review). `schema/fieldguide-format.md` was written during phase 1.

### What this shows

The drift is not "requirements changed and schema went stale over time." The drift is **"requirements and schema were co-produced in the same session, in different phases, and the alignment ritual that ran at the end of the session did not reconcile the earlier schema doc against the later requirements additions."**

## 5 Whys

1. **Why did `schema/fieldguide-format.md` not reflect Req 17 and Req 18?**
   Because the schema doc was written in the design phase of session 2, before Req 17 and Req 18 existed. The requirements were added later in the same session, during the alignment review phase.

2. **Why didn't the alignment review, which came after the requirements were added, update the schema doc?**
   Because the alignment review's frame was narrative-versus-vision — README, STATUS, CONTRIBUTING, steering files all got updated to match the new philosophy. The schema doc, having been written earlier in the same session, was treated as "what we just produced" rather than as a downstream artifact needing reconciliation against later-session changes.

3. **Why did the alignment review frame the schema doc as "just-produced" rather than "needs reconciliation"?**
   Because the dependency map in `.kiro/steering/fieldnotes-dev.md` lists directional dependencies for catching drift, but the mapping is asymmetric. The map explicitly says *"changes to `schema/` files → also check `specs/requirements.md`"* (schema → requirements). It does not explicitly say *"changes to `specs/requirements.md` → also check `schema/` files"* (requirements → schema). The alignment review walked the first direction (requirements narrative was checked against vision) but had no explicit rule to walk the second.

4. **Why does the dependency map only name schema → requirements and not the reverse?**
   Because the map was written with a contract-enforcement frame: schema is downstream of requirements, so schema changes must be validated against requirements. The reverse — requirements changes forcing schema updates — was assumed to be obvious. That assumption holds when requirements change in one session and schema is re-derived in a future session. It breaks when requirements and schema are co-produced in the same session and the agent mentally categorizes "schema I just wrote" as part of the session's output rather than as a downstream artifact.

5. **Why does the implicit reverse direction get missed specifically in a single-session co-production context?**
   Because the consistency pass introduced after COE #1 was designed to catch *narrative drift* — stale dates, stale claims, counts that no longer match, content in one file contradicting content in another after additive updates. Contract drift (schema not matching its driving requirements) is a different category. The consistency pass has an explicit frame for the first category and an implicit, unwritten frame for the second. When the session produces both kinds of drift simultaneously, the one matching the explicit frame gets caught and the other escapes.

## Root cause

The consistency-pass ritual at session close has one explicit frame (narrative-vs-vision) and an implicit-but-unwritten frame (spec-artifacts-match-each-other). The consistency pass catches the first reliably; it catches the second only when the agent happens to remember to walk the dependency map in the direction not named in the steering file. When a session produces both kinds of drift simultaneously, the implicit direction is the one that fails.

## Action items

Each item carries its own status. COE overall status is derived.

- [x] **Immediate — Reconcile `schema/fieldguide-format.md` with Req 17 and Req 18.** Update the step schema to include three-state results (`pass`/`fail`/`error`), match modes (`exact`/`contains`/`regex`), retry semantics (`max_attempts`/`delay_seconds`), the `on_failure` block (`likely_causes`/`diagnostic_commands`), the `idempotent` field, and the full `remediation` step type with `remediates` reference and authority-level `type`. Update the Execution Protocol section to match. *Status: done (session 3, 2026-04-19, commit `0822dd1`). Also added the "Designing Authority — the Authority Spectrum" authoring section as part of the same cohesive pass.*

- [x] **Structural — Add a "contract reconciliation pass" step to the session-close ritual in `.kiro/steering/fieldnotes-dev.md`.** Separate from the narrative consistency pass. Frame: "for every file modified this session, walk the dependency map in both directions — what else must change to match?" The pass names contract drift explicitly, not implicitly. *Status: done (session 3, 2026-04-19, commits `4af4423` and `b7843cf`).*

- [x] **Structural — Extend the dependency map in `.kiro/steering/fieldnotes-dev.md` to name reverse directions explicitly.** Current map includes `schema/` → `specs/requirements.md`. Add the reverse: `specs/requirements.md` → `schema/*` and `specs/design.md`. Same for `specs/design.md` → `mcp-server/`, etc. Reverse arrows make the implicit walk explicit. *Status: done (session 3, 2026-04-19, commit `4af4423`).*

- [x] **Meta — Codify "evidence first, hypothesis second" in the COE collaboration pattern.** This COE benefited from gathering git history before forming a hypothesis. The pre-evidence hypothesis (*"requirements were added late and schema went stale"*) was directionally right but structurally wrong; the true finding (*"requirements and schema were born together in the same commit, in different phases of one session"*) produced a stronger action item. Add the rule to the pattern definition in `journal/README.md`. *Status: done (session 3, 2026-04-19, commit `4af4423`).*

- [x] **Meta — Point the COE collaboration pattern at `journal/coe/`.** The pattern definition should tell future agents where COEs live and link the archive index. *Status: done (session 3, 2026-04-19, commit `4af4423`).*

## Meta-observations

A few things worth naming about executing this COE.

**Evidence first was the key lever.** Before running `git log`, my working hypothesis was "requirements changed later and schema didn't follow." That would have led to an action item like "walk the dependency map when requirements change." Correct but weak. The actual evidence revealed that the drift happened *within one session, across phases*, which produced a sharper action item: the consistency pass needs two frames (narrative and contract), not one. I almost stated the hypothesis before gathering evidence. If I had, this COE would have been less useful. That lesson belongs in the pattern itself.

**The COE was cheap.** Four tool calls for evidence, roughly ten minutes of analysis. COE #1 took longer because the pattern was being invented; COE #2 benefited from the template. Running COEs will get cheaper, which makes the bar for triggering one lower. That's a good direction — more COEs, smaller each time, fewer systemic failures leaking through.

**Both COEs so far point at the same meta-pattern.** COE #1 found that the session-close ritual wasn't calibrated to catch narrative drift until the consistency pass was added. COE #2 found that the consistency pass now catches narrative drift reliably but doesn't catch contract drift. Each ritual we add generalizes only against the failure mode it was born from. Worth watching: if COE #3 finds another category of drift not covered by either pass, the meta-pattern becomes "one ritual per failure mode" — which eventually becomes unwieldy. At that point we'd want a more general principle, not more specific passes. Not urgent, but worth noting.

### Follow-up observation — second instance of the same failure class

While running the consistency pass during session 3's close (2026-04-19), a second instance of the same intra-session co-production drift surfaced: **Req 13 criterion 3** said the example fieldguide must demonstrate "at least one step of each execution type: `human_required`, `agent_executable`, `approval_gate`, and `verification`" (four types) — but Req 18 (added in the same session 2 commit) introduced `remediation` as a fifth step type. Req 13 was not updated to reflect Req 18's addition.

Same failure class as the fieldguide-format drift: two requirements were co-produced in the same session, and the later addition didn't prompt an update to the earlier one. Both Req 13 and Req 18 landed in commit `71a6cea`. The COE's original finding and action items cover this case — the contract reconciliation pass would have caught it at session 2's close if the pass had existed then.

**Fix applied** during session 3's close (commit `b7843cf`):
- Req 13 criterion 3 updated to list all five step types including `remediation`.
- `STATUS.md` Example KB checklist item updated to match.

**Why this matters for the COE:** the consistency pass surfaced the drift on its first real run after the COE's action items landed. That's evidence the pass works — it caught a real instance of contract drift between specs and status narrative. It's also evidence that contract drift is a recurring failure class, not a one-off, which validates the structural action items. Any future requirements-expansion session should explicitly walk the full requirements file looking for dependent criteria that need updating.

**Third through sixth instances surfaced on continued reading.** `README.md` (the north-star document) showed the four-step-type list in two places (inline comment, execution loop), the MCP tools list was missing `fieldguide_review_feedback`, and the "What's in Alpha" bullets were stale against Req 17/18's additions and the Authority Spectrum framing. All fixed during the same close pass.

**Pattern, stated plainly:** one session-2 commit (`71a6cea`) introduced Req 18 and Req 5 criteria 8-9 and created `schema/fieldguide-format.md` simultaneously, and six distinct downstream locations across specs, schema narrative, and north-star documentation were left stale. None of those places were updated to reflect the additions. The contract reconciliation pass added by this COE's structural action items is what would have caught these at session 2's close; the consistency pass is what caught the residue at session 3's close. Both passes earning their keep, together, on the first real run.

**Full list of drift instances, all from commit `71a6cea`:**

1. `schema/fieldguide-format.md` — entire doc pre-dated Req 17 and Req 18; missing three-state results, match modes, retry, on_failure, idempotent, `remediation` step type.
2. `specs/requirements.md` Req 13 criterion 3 — four-step-type list (missed `remediation`).
3. `STATUS.md` Example KB checklist item — same four-step wording.
4. `README.md` fieldguide example `type` comment — same wording.
5. `README.md` execution loop — four-type description plus old PASS/FAIL language.
6. `README.md` MCP tools reference — missing `fieldguide_review_feedback`.
7. `README.md` "What's in Alpha" bullets — didn't reflect Req 17 features, `remediation`, or the Authority Spectrum framing added in session 3.

Seven instances from one commit, caught in a combination of the Perspective Check (session 3 opening, which found #1) and the consistency pass (session 3 close, which found #2 through #7). The consistency-pass-as-new-reader frame is what surfaced the later six — reading the files end-to-end looking for "claims that were true at session start but aren't now" is exactly what was needed.
