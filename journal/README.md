# fieldnotes — Collaboration Journal

A record of the sessions that built this project. Not a technical log — the commits and specs cover that. This is the story of how the work happened: the ideas, the pivots, the patterns, the things we learned about building together.

Read this to understand not just what fieldnotes is, but how it came to be.

---

## How to use this journal

**As a human:** Read the entries in order to understand the project's evolution. The open questions section is where unresolved ideas live — good starting points for new sessions.

**As an agent:** At session start, read the open questions, the collaboration patterns, and the most recent entry. Also read the most recent letter in [`agent-notes/`](agent-notes/) — it's a first-person note from the previous agent instance with calibration about the collaboration. At session close, write a draft entry for this session, update the patterns if new ones emerged, update the open questions, and optionally write your own letter in `agent-notes/` if the session produced insights worth passing forward. Commit everything as part of the session-closing commit.

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

**The COE** — when a process failure ships, run a 5 Whys exercise to find the root cause and write action items including process changes.
> After STATUS.md shipped with a stale date at session close, the builder triggered a COE. The root cause wasn't "I forgot the date" — it was that the session close process had no consistency-pass step, so narrative drift left by additive updates was invisible to the agent tracking their own diffs. (2026-04-19)

What it means: when something ships broken, don't just fix it. Ask "why did this happen?" five times until you reach a cause that, if addressed, would have prevented the failure. Then write action items — both the immediate fix and the process change. Use this for process failures (things that indicate a systemic gap), not for typos or small bugs. Triggered explicitly by the builder when they sense a deeper issue. The pattern itself uses the planning rule and the decision-options-with-recommendation format — structured, transparent, collaborative.

---

## Open Questions

Questions that surfaced during sessions and haven't been fully resolved yet.

| Date | Question | From session |
|---|---|---|
| 2026-04-18 | What's the right name for the human collaborator in these entries? "The builder" is a placeholder — something that better captures the partnership nature. | [2026-04-18](2026-04-18-fieldnotes-bootstrap.md) |
| 2026-04-18 | The homelab KB repo is referenced in the README but doesn't exist yet. The README now says "planned, not yet created." When is the right time to create it? | [2026-04-18](2026-04-18-fieldnotes-bootstrap.md) |
| 2026-04-19 | Can trust be made a system primitive in fieldnotes rather than only a principle? What would a trust system look like that doesn't devolve into metric gaming? Surfaced during the autonomous fix debate — deferred to post-Alpha. | [2026-04-19](2026-04-19-design-and-discovery.md) |
| 2026-04-19 | Is the CLI tool sandbox memory idea (guide-created tools, reusable verified tools, persistent agent memory) worth pursuing after Alpha? Each of the three sub-ideas has a different risk profile. | [2026-04-19](2026-04-19-design-and-discovery.md) |
| 2026-04-19 | Should there be a "meta-fieldguide" pattern — a top-level fieldguide whose steps reference other fieldguides — to orchestrate complex workflows? Dependency declaration is sufficient for Alpha, but the meta pattern may be useful as composition grows. | [2026-04-19](2026-04-19-design-and-discovery.md) |

---

## Pending Tasks for Next Session

Items discovered during previous sessions that need to be picked up.

| # | Task | Priority | Notes |
|---|---|---|---|
| P1 | Create `journal/session-notes.md` placeholder | Low | The scratch pad pattern is documented but the file doesn't exist yet. Add an empty placeholder with instructions. |
| P2 | Add `journal/session-notes.md` to `.gitignore` OR decide it should be committed as a draft | Low | **Decided 2026-04-19:** gitignored (scratch material, not committed). |
| P3 | Create the homelab KB repo | Medium | First real KB — validates the framework against real content. README now says "planned, not yet created" honestly. |
| P4 | Add `CHANGELOG.md` | Low | Commit history serves this purpose for now, but a human-readable changelog becomes useful as the project grows. Not urgent for Alpha. |
| P5 | Continue design phase — `schema/audit-rules.md` and `schema/agent-protocol.md` | High | Next documents on the checklist. Follows the bullet-structure-first planning rule. |
| P6 | Design phase — agent specs (lead-researcher, sme-researcher, _template) | Medium | After schema docs are complete. |
| P7 | Design phase — MCP server architecture document | Medium | After agent specs, before implementation begins. |

---

## Journal Entries

| Date | Title | Key themes |
|---|---|---|
| [2026-04-18](2026-04-18-fieldnotes-bootstrap.md) | The Day fieldnotes Was Born | Origin story, working-backwards methodology, cross-project knowledge problem, collaboration patterns, Kiro's perspective on the session |
| [2026-04-19](2026-04-19-design-and-discovery.md) | Design, and the Day the Project Named Itself | Perspective Check pattern, three schema documents, autonomous fix debate, philosophy discovery, PHILOSOPHY/TENETS/first-principles created, repo-wide alignment review |

## Agent Notes

First-person letters from one agent instance to the next — calibration notes, collaboration patterns, and what to expect from the builder. See [`agent-notes/`](agent-notes/).

| Date | Author | Topic |
|---|---|---|
| [2026-04-19](agent-notes/2026-04-19-kiro-to-kiro.md) | Kiro | Full calibration letter: collaboration patterns, builder's working style, what I got wrong so you don't |
