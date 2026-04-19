# COE — Session-3 Date Drift

**Opened:** 2026-04-19 (session 3, during post-close re-check)
**Status:** Closed
**Last status change:** 2026-04-19

## The failure

Every artifact created during session 3 was initially dated 2026-04-20. The real date is 2026-04-19 (same calendar day as session 2, which happened earlier the same day). The incorrect date was stamped into:

- Three file names (`journal/2026-04-20-calibration-and-closing-the-loop.md`, `journal/agent-notes/2026-04-20-kiro-to-kiro.md`, `journal/coe/2026-04-20-requirements-to-schema-drift.md`)
- Dozens of dated content references inside files (STATUS.md last-updated, session-state.md dates, parking-lot entries, COE opened/status-change, journal entry title, letter signoff)
- Four commit messages (`0822dd1`, `4af4423`, `b7843cf`, `1460280` — all dated 04-19 in git metadata but with 04-20 references in message bodies)
- Cross-references pointing to the misnamed files

Noticed by: Yoel, at the very end of session 3's close, reading STATUS.md.

## Evidence

### System date at the time of the error

```
$ date
domingo, 19 de abril de 2026, 17:39:31 CST
```

Run at the moment the error was discovered. Confirms the real date is 2026-04-19 (Sunday).

### Pattern of the wrong date

Grepping the tree at discovery time surfaced 2026-04-20 in:

```
STATUS.md                                                           (7 instances)
.kiro/steering/fieldnotes-dev.md                                    (2 instances)
.kiro/steering/session-state.md                                     (9 instances)
journal/parking-lot.md                                              (8 instances)
journal/README.md                                                   (10+ instances)
journal/agent-notes/README.md                                       (2 instances)
journal/coe/README.md                                               (2 instances)
journal/coe/2026-04-20-requirements-to-schema-drift.md              (9 instances, now renamed)
journal/2026-04-20-calibration-and-closing-the-loop.md              (1 instance, now renamed)
journal/agent-notes/2026-04-20-kiro-to-kiro.md                      (1 instance, now renamed)
schema/fieldguide-format.md                                         (1 instance in the Changelog line I wrote today)
```

All of the above originated in session 3. The 2026-04-20 dates inside `schema/article-format.md` and the `schema/fieldguide-format.md` example frontmatter block are pre-existing placeholder dates from session 2 (verified via `git log`), not session-3 drift — those are left alone.

### What the commits say

```
0822dd1 2026-04-19 spec: reconcile fieldguide-format.md with Req 17 and Req 18
4af4423 2026-04-19 docs: parking lot + COE archive + contract reconciliation pass
b7843cf 2026-04-19 docs: session close 2026-04-19 — calibration and the ritual auditing itself
1460280 2026-04-19 docs: letter and steering refinements from re-read
```

The commits themselves are git-metadata-dated 2026-04-19 (correct). Their message *bodies* contain 2026-04-20 references (incorrect, reflecting the agent's wrong session-date assumption). The drift is in the message content, not the commit timestamp.

## 5 Whys

1. **Why did session 3 artifacts get stamped with 2026-04-20?**
   Because the agent assumed "next session after session 2" meant "next calendar day after session 2." Session 2 was dated 2026-04-19; the agent inferred session 3 was 2026-04-20 without verifying.

2. **Why did the agent infer the date instead of verifying it?**
   Because the session-start ritual did not include an explicit step to verify the date. The ritual said to read STATUS.md, PHILOSOPHY.md, TENETS.md, requirements, the journal, the letter — all correct — but nothing said "run `date` to establish the current date before writing any dated artifact."

3. **Why didn't the ritual include date verification?**
   Because the need wasn't obvious in advance. The pattern "sessions happen on distinct days" felt like a safe assumption. It isn't. Two sessions can happen on the same calendar day, especially when session 2 ended late-night and session 3 started later the same day. The assumption was unverified and would eventually fail — today was when.

4. **Why was the assumption treated as safe when Tenet 3 ("knowledge has provenance") and the working-backwards discipline both argue for verification over inference?**
   Because dates feel like a low-stakes detail, not a knowledge claim. The agent's attention was on the substantive work — schema reconciliation, picker-default, routine audit — and dating artifacts felt like scaffolding. Scaffolding gets less verification attention than load-bearing content, which is exactly when silent errors land.

5. **Why do low-attention details produce compounding damage when they're wrong?**
   Because dates propagate. One wrong date in a filename breeds wrong references in the index, in the letter, in the session-state, in the COE. Within a few hours the drift was present in over 40 locations. Low-attention details that propagate to many places deserve the same verification discipline as load-bearing content, because their damage is not proportional to their apparent weight — it's proportional to how widely they spread.

## Root cause

The session-start ritual did not require verifying the date. Inference from "previous session was 04-19" to "this session is 04-20" was treated as safe, when it isn't. Low-attention details that propagate widely (dates, names, IDs) need the same verification discipline as load-bearing content, because their damage scales with spread, not with apparent importance.

## Action items

- [x] **Immediate — Rename the three dated session-3 files.** `git mv` to preserve history. New names: `journal/2026-04-19-calibration-and-closing-the-loop.md`, `journal/agent-notes/2026-04-19-kiro-to-kiro-session-3.md` (disambiguated from past-Kiro's 2026-04-19 letter), `journal/coe/2026-04-19-requirements-to-schema-drift.md`. *Status: done (session 3, 2026-04-19, this commit).*

- [x] **Immediate — Fix dated content inside all affected files.** STATUS, session-state, parking-lot, journal README, agent-notes README, COE archive README, the two new files, and the fieldguide-format.md Changelog line I wrote today. Leave pre-existing example dates in schema/article-format.md and schema/fieldguide-format.md example frontmatter (not session-3 drift). *Status: done (session 3, 2026-04-19, this commit).*

- [x] **Structural — Add "verify the date with `date`" to the session-startup ritual in `.kiro/steering/fieldnotes-dev.md`.** Step 10, immediately after reading the most recent letter and before any dated artifact creation. *Status: done (session 3, 2026-04-19, this commit).*

- [x] **Meta — Document the failure pattern for future agents.** Low-attention details that propagate widely (dates, names, IDs) need the same verification discipline as load-bearing content. Named in this COE and referenced from the steering file's new date-verification step. *Status: done (session 3, 2026-04-19, this commit).*

- [ ] **Accept-as-is — Commit messages dated 2026-04-20 stay that way.** Rewriting history on already-pushed commits is destructive and removes the honest record of the error. The messages stay; this COE is the record of why. *Status: decided (session 3, 2026-04-19). Not an open action item, just explicit acknowledgment.*

## Meta-observations

**The COE was triggered by Yoel's outside-view check.** I had completed the full session close, pushed four commits, and declared the session done. Yoel's question "wait, the STATUS date says 2026-04-20" caught a drift I couldn't see from inside. This is the third time this session that an outside-view check from the builder caught something I'd missed (the others: "stuck commit," "you got cut"). The outside view keeps earning its keep.

**The session-start ritual has now grown to 10 steps.** That's significant. The ritual-complexity-watch parking lot item is relevant: we added this step because a real failure demanded it, but the growth is real. The next agent should feel the weight of this ritual and flag it if it's getting heavy.

**The fix was cheap relative to the drift.** About a dozen file edits, three renames, and this COE. Maybe twenty minutes of work. The cost of the error itself was zero to origin (commits stay as-is with honest archaeology), but the cost of discovery-via-Yoel was that I nearly shipped the drift as part of "session 3 closed, ready for session 4" before Yoel caught it. Another few turns and future-Kiro would have inherited the mess.

**Three COEs in one session is not a failure mode — it's the ritual working.** COE #2 was planned (the requirements-to-schema drift we came in to fix). COE #3 was caught by the new passes we'd just added. The growing archive shows the project's errors getting surfaced and resolved *during* sessions, not silently accumulating between them. If future sessions produce zero COEs, check whether we're actually looking for drift or just hoping it isn't there.
