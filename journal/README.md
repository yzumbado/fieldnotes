# fieldnotes — Collaboration Journal

A record of the sessions that built this project. Not a technical log — the commits and specs cover that. This is the story of how the work happened: the ideas, the pivots, the patterns, the things we learned about building together.

Read this to understand not just what fieldnotes is, but how it came to be.

---

## How to use this journal

**As a human:** Read the entries in order to understand the project's evolution. The open questions section is where unresolved ideas live — good starting points for new sessions.

**As an agent:** At session start, read the open questions, the collaboration patterns, and the most recent entry. At session close, write a draft entry for this session, update the patterns if new ones emerged, and update the open questions. Commit everything as part of the session-closing commit.

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

---

## Open Questions

Questions that surfaced during sessions and haven't been fully resolved yet.

| Date | Question | From session |
|---|---|---|
| 2026-04-18 | What's the right name for the human collaborator in these entries? "The builder" is a placeholder — something that better captures the partnership nature. | [2026-04-18](2026-04-18-fieldnotes-bootstrap.md) |
| 2026-04-18 | The `fieldnotes-kb-homelab` repo is referenced in the README but doesn't exist yet. Create before or after the design phase? | [2026-04-18](2026-04-18-fieldnotes-bootstrap.md) |

---

## Pending Tasks for Next Session

Items discovered during the founding session that need to be picked up.

| # | Task | Priority | Notes |
|---|---|---|---|
| P1 | Create `journal/session-notes.md` placeholder | Low | The scratch pad pattern is documented but the file doesn't exist yet. Add an empty placeholder with instructions. |
| P2 | Add `journal/session-notes.md` to `.gitignore` OR decide it should be committed as a draft | Low | Needs a decision: is the scratch pad disposable (gitignore) or a committed draft (tracked)? |
| P3 | Create `fieldnotes-kb-homelab` repo | Medium | Referenced in README, doesn't exist. First real KB — validates the framework against real content. |
| P4 | Add `CHANGELOG.md` | Low | Commit history serves this purpose for now, but a human-readable changelog becomes useful as the project grows. Not urgent for Alpha. |
| P5 | Begin design phase — present bullet structure before writing | High | Next item on the STATUS.md checklist. Schema files, MCP server architecture, agent specs. Use the planning rule. |

---

## Journal Entries

| Date | Title | Key themes |
|---|---|---|
| [2026-04-18](2026-04-18-fieldnotes-bootstrap.md) | The Day fieldnotes Was Born | Origin story, working-backwards methodology, cross-project knowledge problem, collaboration patterns, Kiro's perspective on the session |
