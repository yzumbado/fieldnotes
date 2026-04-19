# Correction of Error — Archive

When a process failure ships — something broken that indicates a systemic gap, not a typo or small bug — we run a COE (Correction of Error). This folder is where each COE lives after it's written.

For the collaboration pattern definition (when to trigger, how to run), see the COE entry in [`journal/README.md`](../README.md#collaboration-patterns).

---

## How to use this folder

**At session start:** scan this index. Any COE in `In progress` state has pending action items — check if this session is the right place to close any of them.

**When running a new COE:** create a new file here named `YYYY-MM-DD-short-title.md`. Use the [template below](#file-template). Open with status `Open`.

**When closing an action item:** update the item's status and record the commit that resolved it. The COE's aggregate status is derived — any pending item means `In progress`; all items done means `Closed`.

**Files stay forever.** COEs are append-only archive. Closed doesn't mean deleted; it means done. Future sessions read closed COEs for pattern recognition.

---

## Status model

| Status | Meaning |
|---|---|
| `Open` | COE is being written, findings are still being reviewed, human has not yet approved action items. |
| `In progress` | Findings approved, action items identified, at least one still pending. This is where most COEs spend their life. |
| `Closed` | All action items complete. The COE is archived as history. |

Transitions:
- `Open` → `In progress` — when findings are approved and action items are locked in.
- `In progress` → `Closed` — when the last pending action item is marked done.

No COE skips `In progress`. Even if all action items are completed in the same session the COE was written, the transition goes through `In progress` before `Closed`. This keeps the lifecycle consistent and visible.

---

## Index

| Date opened | COE | Status | Last update |
|---|---|---|---|
| 2026-04-20 | [Requirements-to-schema drift](2026-04-20-requirements-to-schema-drift.md) | Closed | 2026-04-20 |

---

## File template

```markdown
# COE — [Short Title]

**Opened:** YYYY-MM-DD
**Status:** Open | In progress | Closed
**Last status change:** YYYY-MM-DD

## The failure

What shipped broken. One or two sentences — the specific observable outcome, not the cause.

## Evidence

What we gathered before interpreting anything. Git history, file contents, commit messages, timestamps — whatever shows what actually happened. Evidence first, hypothesis second.

## 5 Whys

1. **Why did [the failure] happen?**
   [Answer grounded in evidence.]

2. **Why did [the cause from #1]?**
   [Answer grounded in evidence.]

3. **Why [...]?**
   [...]

4. **Why [...]?**
   [...]

5. **Why [...]?**
   [Root cause — if addressed, the failure class doesn't recur.]

## Root cause

One or two sentences summarizing what the 5 Whys revealed.

## Action items

Each item carries its own status. COE overall status is derived.

- [ ] **Immediate — [short title].** [Description.] *Status: pending.*
- [ ] **Structural — [short title].** [Description.] *Status: pending.*
- [ ] **Meta — [short title].** [Description, if any.] *Status: pending.*

When an item closes, replace `[ ]` with `[x]`, change *Status: pending* to *Status: done (YYYY-MM-DD, commit `abc1234`)*, and add the commit link.

## Meta-observations

What did we learn about running the COE itself? Any refinements to the pattern to propose?
```
