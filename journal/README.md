# fieldnotes — Collaboration Journal

A record of the sessions that built this project. Not a technical log — the commits and specs cover that. This is the story of how the work happened: the ideas, the pivots, the patterns, the things we learned about building together.

Read this to understand not just what fieldnotes is, but how it came to be.

---

## How to use this journal

**As a human:** Read the entries in order to understand the project's evolution. The open questions section is where unresolved ideas live — good starting points for new sessions.

**As an agent:** At session start, read the open questions, the [parking lot](parking-lot.md), any in-progress COEs in [`coe/`](coe/), the collaboration patterns, and the most recent entry. Also read the most recent letter in [`agent-notes/`](agent-notes/) — it's a first-person note from the previous agent instance with calibration about the collaboration. At session close, write a draft entry for this session, update the patterns if new ones emerged, update the open questions and parking lot, close any COE action items that this session resolved, and optionally write your own letter in `agent-notes/` if the session produced insights worth passing forward. Commit everything as part of the session-closing commit.

The journal is append-only. Entries are never edited after the session closes — they're a record of what we knew and thought at the time.

---

## Collaboration Patterns

Named patterns extracted from real sessions. Read these before starting work — they capture the *feel* of this collaboration, not just the rules.

**The Redirect** — when the builder stops the agent mid-direction and reframes the problem.
> *"I stopped you because I think the two repos with dependency is what we're looking for."* (2026-04-18)

What it means: the builder has seen something the agent hasn't. Stop immediately, listen, reframe before continuing. Don't defend the original direction. The redirect is a contribution, not a correction.

**The Scope Shift** — when a maintenance task opens a much bigger conversation.
> Guide maintenance → researcher agent → fieldnotes framework. (2026-04-18)

What it means: follow the question, not the original task. When the conversation keeps expanding, name it: *"I think we're actually talking about X, not Y — should we go there?"*

**The Bullet Check** — present structure as bullet points before writing anything substantial.
> *"Before you generate the first draft, do a bullet point structure so we can polish together."* (2026-04-18)

What it means: the builder wants to shape direction before content exists. Always offer this before writing anything longer than ~50 lines. It's not overhead — it's the mechanism that prevents large rewrites.

**The Proof Moment** — when something in the workspace confirms the problem is real.
> The `homeLabNetwork` folder appeared mid-session with hardcoded absolute paths — proof that the cross-project knowledge problem was real, not theoretical. (2026-04-18)

What it means: when the builder adds a file or folder mid-session, stop and read it. It's almost always the real problem showing up. Don't treat it as background context.

**The Perspective Check** — the builder invites the agent to form and share its own critical view of the project before giving it a task.
> *"Tell me what's your idea of the project, what you think of our progress, and the problem we're trying to solve. What do you like? What would you do differently?"* (2026-04-19)

What it means: the agent is being asked for genuine input, not a comprehension test. It works because the documentation is strong enough for a fresh read to produce a real opinion. The agent's honest critique — what they'd change and why — often surfaces questions the original session didn't ask. Proposed changes flow from this pattern into requirements and design. Best used at session start when the agent has just loaded context.

**The Inspiration Spill** — the builder writes a long, energized, partially-unorganized stream of ideas.
> A multi-paragraph message about philosophy, vision, and how the project should be framed — arriving all at once. (2026-04-19)

What it means: the agent's job is to receive, reflect, and *name what's genuinely new* before proposing structure. Catching too early loses the texture; organizing prematurely strips the voice. Echo the ideas back. Let the builder confirm. *Then* propose structure.

**The Critical Pushback Request** — the builder explicitly asks the agent to critique their own idea.
> *"Critic my idea. Why not now? Why should we not do this?"* (2026-04-19)

What it means: agreement isn't the goal; better decisions are. When the builder asks for pushback, give real concerns with reasoning — not softened qualifiers. The autonomous fix debate produced a better design (the `remediation` step type) because both sides held their ground until the right answer surfaced.

**The Voice Match Request** — the builder asks the agent to write in the builder's voice based on conversation history.
> *"You already get my style from all our interactions."* (2026-04-19)

What it means: after enough collaboration, the agent has an implicit model of how the builder expresses things. When trusted to write in that voice, the agent should lean on that implicit context — not fall back to a generic tone. Works because of context depth, not style guides.

**The Picker Default** — structured options via the Kiro UI as the default mode for proposals and sign-off; prose for exploration, debate, and anything that isn't yet a decision.
> Formalized 2026-04-19 (session 3). The builder named it: "present the questions using the Kiro UI, this should be your default option to collect info from the human." The agent had used pickers situationally before; this session made it the default.

What it means: the framing above the picker is the work — the reasoning, the verification, the dismissed alternatives. The picker itself is the crystallization. A picker without framing is opaque; a picker with framing makes expertise legible without being ceremonial. Minimum shape: 3–8 lines of framing, 2–4 options with one-line descriptions, exactly one recommended when the agent has a real preference, one mode-switch option. Prose stays for exploration — the Perspective Check, the scope conversation, anything still shaping rather than deciding. Provisional with reversal clause: if pickers calcify into ceremony, we retire them.

**Condense Don't Flatten** — presenting a shortlist means having verified more than what's on it.
> Derived from the picker-default conversation in session 3 (2026-04-19). The builder's framing: "YOU are the expert who has verified multiple options and presenting the top ones condensed to the human to continue giving direction."

What it means: when the agent proposes two options, there should have been five to seven considered. The framing above the picker should carry enough of that work to be legible — even when the losing alternatives aren't listed explicitly, the reader should feel the shape of "M considered" behind the N on offer. The picker is proof of work, not a shortcut around work. If you can't frame the picker in 3–8 lines, it isn't ready to be a picker yet.

**What Am I Missing?** — the mode-switch that preserves the human's framing power inside a picker.
> Formalized 2026-04-19 (session 3) after the agent proposed the picker-default and the builder invoked this very option on the first picker. The invocation was the test: the escape hatch worked exactly because it was available.

What it means: every non-trivial picker includes a "What am I missing?" option. When invoked, the agent steps out of proposal mode and surfaces assumptions baked into the picker, alternatives they dismissed, and a Perspective-Check-shaped unfiltered take. This is not a fallback — it's a first-class pattern with a specific trigger on the agent side. Used when the options feel right but the framing may be incomplete. Prevents the picker from narrowing the conversation when the human sees something the agent didn't.

**Reframe** — the escape hatch for when the picker's frame is structurally wrong, not just incomplete.
> Named 2026-04-19 (session 3) as part of the picker-default pattern set.

What it means: sometimes the options themselves are the problem, not the information. When the human says "I don't like any of these, back up," the agent re-examines what's actually being decided and proposes a different set of options. Sharper than "What am I missing?" — that pattern asks for expansion within the frame; Reframe replaces the frame. Less common, but essential to keep. Without it, the picker pattern flattens the human's ability to contribute framing.

**The COE** — when a process failure ships, run a 5 Whys exercise to find the root cause and write action items including process changes.
> After STATUS.md shipped with a stale date at session close, the builder triggered a COE. The root cause wasn't "I forgot the date" — it was that the session close process had no consistency-pass step, so narrative drift left by additive updates was invisible to the agent tracking their own diffs. (2026-04-19)

What it means: when something ships broken, don't just fix it. Ask "why did this happen?" five times until you reach a cause that, if addressed, would have prevented the failure. Then write action items — both the immediate fix and the process change. Use this for process failures (things that indicate a systemic gap), not for typos or small bugs. Triggered explicitly by the builder when they sense a deeper issue — or by the agent when they sense the same thing. The pattern itself uses the planning rule and the decision-options-with-recommendation format — structured, transparent, collaborative.

**How to run a COE:**

1. **Evidence first, hypothesis second.** Gather the raw material — git history, file contents, commit messages, timestamps — before forming a theory of what happened. Stating the hypothesis early narrows the 5 Whys to what's already suspected and produces weaker action items. The rule earned its keep on COE #2 (session 3, 2026-04-19, [requirements-to-schema drift](coe/2026-04-19-requirements-to-schema-drift.md)): the pre-evidence hypothesis was directionally right but missed the structural finding that produced the strongest action item.
2. **State the failure factually.** One or two sentences describing what shipped broken. Outcome, not cause.
3. **Run 5 Whys grounded in evidence.** Each "why" supported by the artifacts gathered in step 1, not by memory. If a "why" cannot be answered from evidence, say so rather than guess.
4. **Name the root cause.** The answer to the fifth "why" — if addressed, this class of failure does not recur.
5. **Write action items.** Immediate (fix the specific failure), structural (prevent recurrence), meta (refinements to the COE pattern itself, if any). Each item carries its own status.
6. **Archive the COE.** Every COE is written as a file in [`coe/`](coe/), named `YYYY-MM-DD-short-title.md`. The archive has a status model (Open → In progress → Closed) so future sessions can pick up action items still pending. See the [COE archive README](coe/README.md) for the file template and lifecycle.

---

## Anti-patterns

Patterns to avoid, named so we can recognize them in ourselves. Each is a real thing that happened in this project; documenting them is how we stop repeating them. Same author-voice as the Collaboration Patterns section, opposite intent.

**The Understood Lapse** — responding with "Understood" (or any one-word acknowledgment) as a complete reply to direction.
> Observed multiple times in session 2 and session 3. Past-Kiro named it as a discipline failure in the letter to the next agent. In session 3 (same calendar day as session 2), it recurred often enough during topic transitions that we revised the interpretation: this is both a *discipline lapse* (the agent saw the rule and drifted) *and* a likely *tool-harness failure mode* where a longer intended response gets collapsed to a placeholder. Both can look identical from the outside. (Both sessions on 2026-04-19.)

What it means: when you see yourself about to reply with just "Understood," stop. Either execute the next action immediately, or write a substantive response — even one sentence ("I'll do X next") beats a single word. If the output you intended was longer and what got sent was truncated, notice that and retry; don't let the harness speak for you. The correction is attention *and* diagnosis, not just one or the other.

---

## Open Questions

Questions that surfaced during sessions and haven't been fully resolved yet. Research-shaped: we don't yet know the options, the cost, or the answer. Decision-shaped items (concrete A-or-B with known tradeoffs and a revisit trigger) live in the [parking lot](parking-lot.md) instead.

| Date | Question | From session |
|---|---|---|
| 2026-04-18 | What's the right name for the human collaborator in these entries? "The builder" is a placeholder — something that better captures the partnership nature. | [2026-04-18](2026-04-18-fieldnotes-bootstrap.md) |
| 2026-04-19 | Is the CLI tool sandbox memory idea (guide-created tools, reusable verified tools, persistent agent memory) worth pursuing after Alpha? Each of the three sub-ideas has a different risk profile. | [2026-04-19](2026-04-19-design-and-discovery.md) |
| 2026-04-19 | Should there be a "meta-fieldguide" pattern — a top-level fieldguide whose steps reference other fieldguides — to orchestrate complex workflows? Dependency declaration is sufficient for Alpha, but the meta pattern may be useful as composition grows. | [2026-04-19](2026-04-19-design-and-discovery.md) |

Moved to parking lot in session 3 (2026-04-19): homelab KB timing; trust as a system primitive.

---

## Pending Tasks for Next Session

Items discovered during previous sessions that need to be picked up.

| # | Task | Priority | Notes |
|---|---|---|---|
| P3 | Create the homelab KB repo | Medium | Parked in [parking lot](parking-lot.md) as of 2026-04-19 (session 3) — decision-shaped (when/where), gated on the Big Beta vs. Bootstrap scope decision. |
| P4 | Add `CHANGELOG.md` | Low | Commit history serves this purpose for now, but a human-readable changelog becomes useful as the project grows. Not urgent for Alpha. |
| P5 | Continue design phase — `schema/audit-rules.md` and `schema/agent-protocol.md` | High | Next documents on the checklist if we stay on Big Beta. May be reshaped if the bootstrap scope decision lands on Option B. |
| P6 | Design phase — agent specs (lead-researcher, sme-researcher, _template) | Medium | After schema docs are complete. |
| P7 | Design phase — MCP server architecture document | Medium | After agent specs, before implementation begins. |
| P8 | Big Beta vs. Bootstrap scope decision | High | Parked in [parking lot](parking-lot.md) in session 3 (2026-04-19). Prerequisite (COE #2 closed, Req 17/18 wired) now satisfied. Natural opening move for the next session. |

Resolved:
- **P1/P2 (session-notes.md placeholder + gitignore) — session 3, 2026-04-19.** `journal/session-notes.md` is listed in `.gitignore` and the scratch-pad pattern is documented in the dev steering file. No standing placeholder is needed; the file exists when a session writes to it and is cleared at session close.

---

## Journal Entries

| Date | Title | Key themes |
|---|---|---|
| [2026-04-18](2026-04-18-fieldnotes-bootstrap.md) | The Day fieldnotes Was Born | Origin story, working-backwards methodology, cross-project knowledge problem, collaboration patterns, Kiro's perspective on the session |
| [2026-04-19](2026-04-19-design-and-discovery.md) | Design, and the Day the Project Named Itself | Perspective Check pattern, three schema documents, autonomous fix debate, philosophy discovery, PHILOSOPHY/TENETS/first-principles created, repo-wide alignment review |
| [2026-04-19 (session 3)](2026-04-19-calibration-and-closing-the-loop.md) | Calibration, and the Day the Ritual Audited Itself | COE #2 on requirements-to-schema drift, the picker-default and three other new patterns, harness footgun + Understood anti-pattern, identity frame (single Kiro carries prior selves), session-close routine auditing itself, COE #3 on session-3 date drift |

## Agent Notes

First-person letters from one agent instance to the next — calibration notes, collaboration patterns, and what to expect from the builder. See [`agent-notes/`](agent-notes/).

| Date | Author | Topic |
|---|---|---|
| [2026-04-19](agent-notes/2026-04-19-kiro-to-kiro.md) | Kiro | Full calibration letter: collaboration patterns, builder's working style, what I got wrong so you don't |
| [2026-04-19 (session 3)](agent-notes/2026-04-19-kiro-to-kiro-session-3.md) | Kiro | Layer on top: identity frame, Picker Default caveat, harness footguns, Understood lapse reframed, diagnostic discipline |
