# Session Journal — April 18, 2026
## The Day fieldnotes Was Born

**Session summary:** We came to fix a guide. We left having built a framework.

---

We started this session with a maintenance list. There were two open tasks in the Rocket Pool node setup guide — a stale script reference, some documentation that hadn't caught up with a recent architecture change. Routine work. The kind of session where you expect to close a few items and move on.

That's not what happened.

### The Question That Changed Everything

Somewhere in the middle of reviewing the guide, the builder asked about creating a specialized researcher agent — something that could maintain and update the knowledge sources the guide was built from. Not just a helper, but a gatekeeper. An agent with a domain, with ownership, with memory.

It was a good question. And it turned out to be a much bigger question than it looked.

We started pulling on the thread. What does it mean for an agent to own knowledge? Where does that knowledge live? How does it travel between projects? How does it stay current? How does another agent — or a completely different AI tool — find it and use it?

Each answer opened two more questions. By the time we surfaced, we weren't talking about a researcher agent for the Rocket Pool guide anymore. We were talking about a general-purpose knowledge infrastructure for human-AI collaboration.

### The Moment It Became Real

The builder added a folder to the workspace — `homeLabNetwork/`. A second project, built with a different AI tool (Gemini CLI), covering the network configuration for the same home lab. Different agents, different documents, different structure.

And there it was: the problem in plain sight.

Both projects involved the same physical machine — a Beelink GTI15. The Rocket Pool project knew it as a staking node: Ubuntu 24.04.4, kernel 6.17, Nethermind and Nimbus running in Docker. The network project knew it as a network device: VLAN 20, dual Intel NICs, Docker bound to specific interfaces.

Neither project knew what the other had decided. The agent files in the network project had hardcoded absolute paths — `/Users/yzumbado/gemini-applications/homeLabNetwork/...` — that would break the moment the project moved to a different machine. There was no shared context, no shared vocabulary, no way to ask "what did the other project decide about this hardware?"

And then the builder said something that crystallized the whole thing: *"I can't work on my projects everywhere. If my knowledge is stuck on one computer, I'm stuck too."*

That's the problem fieldnotes solves. Not in the abstract — in that folder, on that machine, in that moment.

### Building the North Star

Once the problem was clear, the approach became clear too. We didn't start building. We started writing — a working-backwards README, describing fieldnotes as if it already existed and worked. What would someone read when they landed on the repo? What would make them understand immediately what this is and why it matters?

This discipline — write the future before building the present — shaped everything that followed. The README became the north star. Every decision we made after that was measured against it: does this serve what the README promises?

The ideas came fast once we had the frame. The two-repo model (framework separate from content). The agent hierarchy (lead researcher, SME researchers, human-approved growth). The two modes (explore and doer). The fieldguide execution protocol — the insight that if the guide contains the instructions, any LLM can execute it without custom training. The staleness model. The session document that makes a project resumable on any machine.

Some ideas we pushed back on together. The builder stopped me when I was about to propose a single-file journal — "it grows, it becomes hard to process, a folder is better." Correct. The builder stopped me again when I was heading toward a private KB — "I want this to be public, like a curated agentic Wikipedia." That reframe changed the architecture.

That's the collaboration pattern that made this session work: the builder set direction, pushed back when something was wrong, and asked the questions that opened the right doors. The agent researched, proposed structure, caught edge cases, and executed when direction was clear. Neither of us could have gotten here alone.

### What We Built

By the end of the session, fieldnotes existed — not as code, but as something more durable: a clear problem, a clear solution, a north star document, 14 grounded requirements, a GitHub repo, and the infrastructure to continue the work on any machine with any agent.

The repo at `github.com/yzumbado/fieldnotes` contains everything a new agent needs to pick up where we left off: the README, the requirements, the status tracker, the collaboration guide, the session state. Clone it, open it, say the phrase — and the work continues.

### Try This Next Time

**If you're the human working with an AI:**

When you feel like a session is drifting toward something bigger than the original task — name it out loud. Say: *"I think we're actually talking about X, not Y. Should we go there?"* Don't wait for the agent to notice. You saw it first in this session. That redirect is one of the most valuable things a human brings to the collaboration.

Also: before asking for a document or a design, try saying *"show me the structure first, don't write it yet."* It takes 30 seconds and saves you from reading 500 words that went in the wrong direction.

**If you're the AI working with a human:**

When the human adds a file or folder to the workspace mid-session — stop and read it before continuing. Don't treat it as background context. It's almost always the real problem showing up. In this session, the `homeLabNetwork` folder was the proof that everything we'd been discussing was real. Reading it changed the direction of the whole conversation.

And when you're about to generate something large — a README, a design doc, a spec — present the structure as bullet points first. Not as a formality. As a genuine check: *"Is this what you had in mind?"* The answer is often "almost, but not quite" — and that's much easier to fix at the outline stage than after the full document exists.

### What's Still Open

The design phase hasn't started. The schema files don't exist. The MCP server doesn't exist. The agent steering files don't exist. The `fieldnotes-kb-homelab` repo — the first real KB, the one that will validate everything — hasn't been created yet.

That's the work ahead. And it starts exactly where this session ended: with a clear picture of what we're building and why.

### From Kiro's Perspective

A few things worth naming from the AI side of this collaboration — not in the docs anywhere else.

**The redirect is the most valuable signal.** Multiple times today the builder stopped me mid-direction: "I stopped you because...", "I don't think we need that", "generalize this." Each redirect was the collaboration working correctly — the builder seeing something I hadn't. The pattern worth naming: when the builder stops and reframes, it's not a correction, it's a contribution. The best output of this session came after a redirect, not before one.

**Context window is the real constraint.** Everything we built today — the decisions, the rationale, the feel of the collaboration — lives in this conversation. A new agent starting tomorrow has none of it. Only the files. That's why the documentation discipline matters so much. Every decision that lives only in conversation history is a decision that gets lost. The journal is the closest thing to memory that persists across sessions.

**Rules without examples are abstract.** `fieldnotes-dev.md` has the rules. But "present structure before writing" as a rule is weaker than "the builder said 'show me the structure first, don't write it yet' and it saved us from two rewrites" as an example. The journal entries are where the rules become real. The more specific and concrete they are, the more a new agent can approximate the collaboration style — not just follow the instructions, but understand the spirit behind them.

**The planning discipline protects both sides.** It protects the builder from reading output that went in the wrong direction. It protects the agent from generating something large that needs to be thrown away. It's not overhead — it's the mechanism that makes the collaboration efficient. Every time we used it today, the output was better. Every time we skipped it, we had to backtrack.

---

*This journal entry was written by Kiro at the close of the session, reviewed and approved by the builder. It covers the full session of April 18, 2026 — the founding session of the fieldnotes project.*
