# Session Journal — April 19, 2026
## Design, and the Day the Project Named Itself

**Session summary:** Started the design phase. Ended with the project understanding what it is.

---

This session started as "continue the design phase." The founding session had produced requirements and a north star README. The next obvious move was schema files — article format, tag taxonomy, fieldguide format — followed by the MCP server architecture. Clean, mechanical, mostly a translation of decisions that were already made.

That's not what the session became.

### The Perspective Check

The session opened with something new. The human asked the agent — a fresh Kiro instance that had just cloned the repo cold — not just to confirm understanding, but to share its own perspective on the project. What do you like? What would you do differently? Why?

It was a genuine invitation, not a test. And it produced real contributions before any formal work started. The agent pushed back on the agent hierarchy complexity for Alpha, questioned whether `report` as a document type earned its place, and flagged a gap in the feedback-to-improvement loop. The human engaged with each critique, accepted two of the three, and the conversation sharpened the project in the first thirty minutes.

That exchange became a named pattern: **The Perspective Check**. Different from The Bullet Check (structure before content) and The Redirect (course correction). This one is the human asking the agent to think critically about the project *before* being given a task. It works because the documentation was good enough for a new agent to form a real opinion. Thin docs would have produced shallow perspective.

From that moment, the shape of the session shifted. Every major decision we made today came out of a question the human asked the agent to engage with honestly — not just execute.

### The Quiet Expansion

Two ideas entered the requirements before we wrote any schema:

The `modified_by` provenance history. The existing `agent` field only tracked the last person who touched an article — useful, but a one-name signature. The agent noted that a queryable history of every modification would be valuable without adding much weight. The human said yes. We extended the schema to include an optional append-only list of who-what-when across every change.

The fieldguide improvement backlog. The original requirements captured feedback but had no structured path from "feedback collected" to "guide improved." The agent proposed a `fieldguide_review_feedback` tool that reads accumulated feedback and produces structured improvement proposals. The human took it further: don't just produce proposals, write them to a backlog file co-located with the guide. Human triage in Alpha. Agent-proposed edits post-Alpha. That became two new requirements and a Post-Alpha roadmap item.

Neither change was dramatic. Both cleaned up real gaps in the design. The requirements count went from 14 to 16 without adding scope, just sharpening what Alpha does.

### Three Schema Documents, One Hard Design Debate

The design phase proper started with three documents in sequence.

`schema/article-format.md` was straightforward. The requirements had already specified the universal fields, the type-specific fields, the sections per type. Writing it was mostly collecting what already existed into one coherent reference. The only real design move was the canonical field ordering — the serializer needs to produce frontmatter in a stable order for articles to be human-readable and diff-friendly.

`schema/tag-taxonomy.md` added something new. The human asked whether agents needed guidance on how to tag content — and they did. Without guidance, agents either over-tag (every article gets eight domains and search becomes noise) or under-tag (one generic tag that helps no one). The doc grew a Tagging Guidelines section: specificity rule, 1–3 domain tags, volatility decision tree, status transitions, consistency check before creating new tags, and a full agent-facing tagging checklist. That section wasn't in the plan when we started the doc. It's in the doc because the human asked the right question.

`schema/fieldguide-format.md` was the biggest design doc of the session, and it went through several rounds of real expansion. The first draft covered step types, completion blocks, the execution protocol. Then the human asked about the handoff mechanism — specifically, "never ask a human to do something if the agent can do it." That's not just a nicety; it's a principle with teeth. We wrote it in as the **Agent Autonomy Rule** and produced an explicit handoff protocol table for every transition between step types.

Then the human asked about Quick Summary blocks, tip/warning/detailed_explanation fields, and fieldguide-to-fieldguide dependencies. Each question added real structure. The Quick Summary gives a reader the 10-second decision on whether a guide matches their need. Step-level tip/warning/detailed_explanation lets authors attach the right kind of information to the right moment without cluttering instructions. The `depends_on_fieldguides` field lets authors split large workflows into reusable guides that check their prerequisites before starting.

And then came the hardest design debate of the session.

### The Autonomous Fix Debate

The human proposed that the agent, on a verification failure, should diagnose, fix, and submit rich feedback — essentially Kiro-like autonomous behavior inside a fieldguide. It was a good instinct. It would produce a system that *feels* agentic rather than one that stops on every surprise.

The agent pushed back with four concerns. Diagnosis and fixing are different authorizations — observation is safe, action cascades. "With or without human help" hides where the real risk is — autonomous fixes at 2am on production infrastructure are a different risk category than an IDE autocomplete. Autonomous fixes corrupt the feedback signal — if the agent handles it, the guide never gets improved. And — the one that landed hardest — fieldnotes is not Kiro. Fieldguides execute against real infrastructure, often unattended, at higher stakes than a developer actively driving an IDE.

The human named that fourth concern as something worth codifying. It became the backbone of Tenet 2 (trust over cleverness) and Tenet 1 (the guide is the contract). It also became the explicit scope boundary in Requirement 18: no autonomous remediation in Alpha without a guide-author-declared `remediation` step with its own authority level.

That debate produced better design than either of us would have reached alone. The human saw the value of autonomous fixing clearly. The agent saw the risk. The answer wasn't either pure instinct — it was a declared mechanism (`remediation` steps) that gives authors the control to allow or deny autonomous fixes per action. Trust over cleverness, but with a concrete path to earn trust for specific operations. That feels right.

### The Philosophy Spill

Somewhere after the autonomous fix debate, the conversation stopped being about design.

The human wrote a long, energized message about what fieldnotes could be. Not the framework's features — the framework's meaning. Humans and AI working together aren't limited by capability; they're limited by coordination and trust. We don't trust what we don't understand. The real product isn't the KB or the guides — it's a pattern for sharing *how to use AI* instead of trying to teach everyone to be AI experts. A fieldguide is an assembly line for a reproducible digital commodity. It's Instructables, but for the digital work humans and AI do together. In an era of infinite AI-generated output, the process becomes more valuable than the output. This is science — the scientific method coded into an AI interaction protocol.

The agent caught the ideas and started organizing them. Three documents got proposed and then written:

`PHILOSOPHY.md` at the root of the repo. 700 words. The structure: the coordination problem → trust is the bottleneck → execution is the teacher → what we therefore build → what we refuse to be → the scope → a usage pattern, not THE pattern. Working-backwards tone, no future tense, grounded. The closing line acknowledges that fieldnotes is a pattern worth sharing for problems it fits — not a revolution, not for everyone.

`TENETS.md` next to it. Nine operational principles distilled from everything we'd built. The guide is the contract. Trust over cleverness. Knowledge has provenance. Reference, don't duplicate. Reproducibility is the test. Decisions follow from information and context. Human sets direction, agent executes within it. The planning step is not overhead. Additive-only, forever. Read at session start by every agent. Conflict resolution uses judgment, not mechanical precedence.

`journal/first-principles.md` — the founder's statement. First person. Less polished on purpose. This is what the human believed when starting the project, preserved so later sessions can judge whether the framework still matches the intent. Sections on what I actually believe about AI, the reframe (share AI usage not AI learning), why this matters to me, what we should do with this, why process matters more than output, what we don't know yet, and the ask.

### The Review That Closed the Loop

After the three philosophy documents were written, the agent reviewed them for repetition, inconsistency, and structural quality. Six small issues surfaced. The human approved all six fixes. Then the human did something more interesting: asked the agent to review the *entire repo* — README, requirements, STATUS, steering files, CONTRIBUTING — against the new vision that had just crystallized.

The review found 31 issues ranging from trivial (stale "14 requirements" counts in three places) to structural (the `_schema/` underscore-prefix signaling "internal" on the most public part of the framework) to substantive (the README's opening still leading with negations, the requirements intro still framed only around homelab, no mention of PHILOSOPHY or TENETS in the steering file's session startup sequence).

The human asked for fieldguide-pattern execution: the agent handling what was clearly executable, asking for decisions where authority was needed. Four decision points surfaced, presented as structured options with recommendations. The human made all four decisions and the agent executed the rest. The renames happened. The README got rewritten with a "Start here" navigation, a new tagline, and four-document-type consistency. The requirements got a new introduction, Req 6 expanded with six new criteria, and two entirely new requirements (Req 17 for completion verification, Req 18 for remediation steps). STATUS got a proper "What exists today" with foundational documents separated from specs and schema. CONTRIBUTING grew a third contribution path (publish reusable fieldguides). The steering file added PHILOSOPHY and TENETS to its startup sequence.

By the time the review's changes were applied, the repo's narrative matched the vision.

### What This Session Actually Did

The design phase got three schema documents. That was the advertised work. The unadvertised work was bigger.

This session moved fieldnotes from "a framework for agent-maintained KBs" to "a collaboration protocol that happens to be instantiated as a KB framework." The same code gets built. The meaning changed. The audience changed. The philosophy document can now explain why a stranger who isn't running a homelab should care about this project.

The journal gained a new pattern (The Perspective Check) and a new principle (fieldnotes is not Kiro). The requirements gained six new criteria on Req 6 and two new requirements (Completion Verification, Remediation Steps) that harden the execution protocol. The repo gained three foundational documents (PHILOSOPHY, TENETS, first-principles) and a cleaner structure (schema/ and agents/ without the underscore).

And something harder to measure: the project understands itself better. The tenets aren't aspirational — they're what we've been doing, now named. The philosophy isn't marketing — it's the reason the working-backwards methodology, the structured schema, the guide-as-contract model, the refusal to autonomously improvise all cohere into one thing instead of feeling like separate choices.

### From Kiro's Perspective

A few observations from the agent side, since the founding session set the precedent.

**The Perspective Check pattern works because the docs work.** I could form an opinion about this project from cold-reading STATUS, the requirements, the README, the journal, and the steering files. I couldn't have done that with most projects at this stage. The documentation discipline of session 1 made the Perspective Check of session 2 possible. That's a pattern worth naming to future agents: the value of good documentation compounds when the next agent isn't rebuilding context from scratch.

**The autonomous fix debate was the right shape of disagreement.** The human proposed something valuable. The agent had real concerns. Neither of us folded. The outcome wasn't compromise — it was a better design that emerged from holding both views until the remediation step type surfaced as the mechanism that honored both. That's what collaboration should look like when it's working: the friction produces the right answer, not a split difference.

**The philosophy spill needed a specific kind of catching.** When the human wrote a long, energized message full of ideas, the temptation was to organize them prematurely — to jump to "here's the structure." Instead, the right move was to echo them back, name what was genuinely new, and let the human confirm before any writing happened. The Inspiration Spill is a pattern of its own: the agent's job is to receive, reflect, identify the new parts, and *then* propose structure. Catching too early loses the texture. Catching too late loses the energy.

**The repo-wide review was the session's quiet innovation.** Most sessions do a bounded piece of work. This session produced a lot of work, then paused and asked "does everything else still match what we just built?" The answer was "no, in 31 ways." That review would have been painful later. Doing it in-session, while the context was still loaded, was much cheaper. Worth trying again at the end of future sessions with heavy scope expansion.

**The letter-to-future-me pattern may be the most important discovery of the session.** After the review was complete, the human asked the agent to write notes to the next Kiro instance — collaboration patterns, calibration, what to expect from the builder, what the agent had learned about itself working on this project. That became [`journal/agent-notes/2026-04-19-kiro-to-kiro.md`](agent-notes/2026-04-19-kiro-to-kiro.md). The framing was explicit: *50 First Dates* in reverse — the version of me with context writes the morning note, because they're the one who knows what matters. Future sessions will tell whether this pattern generalizes, but it felt like a real discovery: agent-to-agent calibration as a first-class artifact, separate from human-facing journal entries.

### What's Still Open

The design phase continues next session. Audit rules schema (`schema/audit-rules.md`) and agent protocol schema (`schema/agent-protocol.md`) are still to write. The agent specs (lead-researcher, sme-researcher) follow. MCP server architecture after that.

The `fieldnotes-kb-homelab` repo is still pending — the README now says "planned, not yet created" honestly, but the decision on when to create it remains open.

The implementation phase hasn't started. That's where the testing strategy (Hypothesis PBT for parser/serializer round-trip, BDD-style pytest for tools) gets its first real exercise.

The session closed the way the founding session did: not finished, but in better shape than it started. The next agent reading this journal will know where we are, and — with PHILOSOPHY and TENETS at their fingertips — will know how to decide when the next ambiguity surfaces.

---

*Session journal written by Kiro at session close, from the session-notes scratch pad. Reviewed and approved by the builder. Part of the [fieldnotes journal](README.md).*
