# fieldnotes — Philosophy

> The beliefs this framework is built on. If you want to understand the *why* of fieldnotes, start here.

---

## The coordination problem

Humans and AI are not limited by what they can do. They're limited by how well they can work together.

The capabilities on both sides are already extraordinary. A skilled AI practitioner, paired with a capable model, can research complex topics, write working software, execute multi-step technical workflows, and produce outputs that would have taken days in hours. The ceiling isn't the model. The ceiling isn't the human. The ceiling is the coordination between them — how the work gets organized, how decisions get made, how knowledge moves from one session to the next.

The actual bottleneck is communication bandwidth. Most people don't have the skill to get the most out of AI — not because they lack intelligence, but because they haven't learned the communication pattern. And teaching that skill to everyone doesn't scale. AI evolves faster than humans can learn to use it. Every "AI literacy" curriculum is out of date before it ships.

Most AI collaboration today is ephemeral. It lives in one conversation, on one machine, with one pair of participants. The model's context window is the memory. When the session ends, the knowledge goes with it. The next session starts cold. The next person starts from scratch. The work is real, but it doesn't compound.

fieldnotes takes a different angle on the problem. Instead of trying to teach everyone how to use AI, fieldnotes lets the people who already know how to use AI package their expertise into runnable artifacts. The expert encodes the communication pattern once; everyone else executes it.

---

## Trust is the bottleneck

We don't trust what we don't understand. That's not a bug in human psychology — it's how humans have always decided who to work with.

The reason most people's AI collaboration stays shallow is that the process is opaque. You ask for something, an output arrives, and you have to decide whether to trust it. Without seeing how the work was done — what was researched, what was considered, what was tested — the only signal you have is the output itself. That's not a foundation for trust. That's a coin flip.

fieldnotes makes the process visible. Every knowledge article has a source and an access date — you can check it. Every guide has a structure that tells you exactly what it will do and who will do it — you can audit it. Every session records the decisions made and the issues encountered — you can trace it. The agent that modified an article signs it. The guide that was executed leaves a record of how it went.

When you run a fieldguide, you're not just trusting the output. You're trusting the practice that produced the guide: the research in the knowledge articles, the testing in the feedback, the rationale in the changelog. Trust earned this way compounds — it's portable across sessions, across projects, across humans.

---

## Execution is the teacher

You don't learn the value of human-AI collaboration by reading about it. You learn it by running a guide that compresses a hard day of work into a predictable hour.

A homelab setup that would have taken a weekend of Googling, false starts, and debugging — executed in ninety minutes, with every step verified, every decision recorded, every open question surfaced. The first time that happens, the value stops being theoretical. The compressed feedback loop is the teacher. You see what's possible, and you start asking what else could be done this way.

This is how fieldnotes transfers capability. A human who has figured out a complex workflow captures it in a guide. Another human runs that guide safely, learns by doing, and eventually contributes their own guides back. The experienced practitioner's skill becomes infrastructure the next person stands on. Not a tutorial. Not a course. A working, executable, auditable artifact.

Think of a fieldguide as an assembly line for a specific digital output. Video edited to a format. OS configured to a standard. Research compiled and cited. Game prototyped. System documented. The output is the commodity; the guide is the production line. Humans with very different backgrounds can execute activities from other roles, not by learning those roles from scratch, but by running guides authored by people who already have that expertise.

Models don't matter for this, by design. The guide is protocol-level. Any MCP-compatible LLM can execute a fieldnotes guide. The value doesn't depend on the best model, the most expensive subscription, or the deepest prompt-engineering skill. It depends on the guide being good — and the guide becoming good through use.

---

## What we therefore build

- Structured knowledge with provenance. Every fact has a source. Every article has an owner. Nothing appears from nowhere.
- Guides that are protocols, not prose. Each step declares its execution type, completion condition, and boundaries. The agent knows what to do because the guide tells it precisely.
- Sessions that preserve decisions. Not a log of what was said — a record of what was decided, so the next session or the next machine can continue without losing context.
- Agents that execute within declared boundaries. The guide is the contract. Autonomy is declared per action, not assumed. Cleverness is not a substitute for predictability.
- A two-repo model. The framework is ours. Your knowledge is yours — public or private, portable, never locked in.

---

## What we refuse to be

- Not an autonomous agent framework. Agents in fieldnotes execute contracts; they don't write their own.
- Not a knowledge generator. Every claim is traceable to a source. Nothing is produced "from the model's general knowledge."
- Not a personal KB competitor. fieldnotes works for personal use, but it was designed for sharing — the value is social.
- Not a model race. The protocol is the moat. Any compatible client can execute any guide.
- Not a replacement for judgment. The human sets direction, makes decisions, and validates outcomes. The agent handles the grind. Neither works as well alone.

---

## The scope

Any repeatable, partially-automatable work where human judgment is the bridge and AI can absorb the grind.

A homelab setup. A content production workflow. Extracting value from disconnected systems in an organization. Setting up development environments. Running technical audits. Executing migrations. Anywhere a human has figured out how to make something work and wants that knowledge to survive beyond one session — fieldnotes is the shape that survival takes.

The framework doesn't pick a domain. The guides do.

---

## A usage pattern, not THE pattern

fieldnotes is one way to work with AI. It isn't the only way. There are other patterns — direct collaboration, fully autonomous agents, narrow tools for specific tasks — and they each serve their purpose.

What fieldnotes claims is narrower and stronger: for complex, reproducible, partially-automatable work, structured human-AI collaboration with trust built on visibility is a pattern worth sharing. The value compounds when humans publish their guides, agents execute them reliably, and the resulting work is auditable by anyone who cares to look.

When AI can generate infinite output, the output itself is cheap. What's valuable is the *process*. Every fieldnotes artifact comes with its production record.

The closest existing analogy is Instructables: people share the steps to physically build things, so anyone can reproduce the work. fieldnotes does the same for digital work, but with AI carrying part of the load and a protocol ensuring reproducibility.

If this matches how you want to work with AI, the framework is here. If it doesn't, that's fine — the pattern isn't for every problem. But for the problems it fits, it fits well.

---

## See also

- [TENETS.md](TENETS.md) — the operational principles derived from these beliefs.
- [journal/first-principles.md](journal/first-principles.md) — the founder's statement in first person. Less polished, more personal.

---

*fieldnotes is built in the same model it describes: a human and an AI working together, documenting the process, earning trust through visible practice, shipping a framework that anyone can audit, fork, and improve.*
