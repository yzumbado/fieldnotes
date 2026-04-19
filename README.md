# fieldnotes

**A framework for making human-AI work reproducible, transferable, and auditable.** Built on a structured knowledge schema and a protocol any LLM can execute.

![Alpha](https://img.shields.io/badge/status-alpha-orange) ![License](https://img.shields.io/badge/license-MIT-blue)

> This is Alpha. It works. It is not finished. We are building it by using it.

> **Development note:** This README is a working-backwards document — written as if Alpha is complete, to define what we're building before we build it. It is the north star, not the current state. For real implementation status, see [STATUS.md](STATUS.md).

---

## Start here

Three entry points depending on what you're looking for:

- **[README.md](README.md)** (this file) — what fieldnotes is and how to use it
- **[PHILOSOPHY.md](PHILOSOPHY.md)** — why fieldnotes exists, what it believes about human-AI collaboration
- **[TENETS.md](TENETS.md)** — the operational principles that govern decisions in this project

For the founder's statement in first person, see [journal/first-principles.md](journal/first-principles.md).

---

## What is fieldnotes?

fieldnotes is a system for human-AI work that compounds — work another person, another session, another AI tool can pick up, reproduce, verify, and improve.

It is a structured knowledge schema with provenance. A fieldguide format that turns procedures into executable protocols any MCP-compatible LLM can run. An MCP server that exposes both as tools. Agent steering files that encode how a researcher thinks.

It is not a database. It is not a scraper. It does not generate knowledge from nothing. It is a system for knowledge that was actually earned — through research, through building things, through making mistakes and figuring out why. Every fact has a source. Every article has an author. Every guide has a protocol that any AI can follow. And all of it is organized so any compatible agent, on any machine, can pick up where the last one left off.

You bring the knowledge. fieldnotes gives it a home — and a way to use it.

---

## Where this came from

This didn't start as a framework. It started as a problem.

I was building a complex technical project with an AI assistant as a genuine collaborator — not just answering questions, but researching, writing, and executing alongside me. I had one workspace for the software installation and configuration, another for the network setup. Each had its own documents, its own agent context, its own accumulated decisions.

Then I needed to change how the software was distributed across the physical machines. A decision that lived in the first workspace had direct consequences for the second. And I had no way to move it.

Not because the knowledge didn't exist — it did. It lived in conversation history, in notes files, in my head. But it wasn't in a form that could travel. When I opened the network workspace, it didn't know what the software workspace had decided. When I started a new session, the agent reconstructed context from scratch. When something changed upstream, I found out downstream when something broke.

The knowledge existed. It just had no home — and no way to cross a boundary.

I tried the obvious things. A flat markdown file of sources. Notes in the project README. A running doc of decisions. None of it was maintainable across projects. None of it was something an agent could actually navigate — it was written for me, in the moment, not for the next session or the next project that needed to know what this one had decided.

Then I realized the problem was deeper than organization. I wasn't just losing knowledge between projects. I was losing the *context* that made the knowledge useful — the decisions, the rationale, the things we tried that didn't work, the open questions we hadn't answered yet. Every new session started cold. Every new project started from scratch.

fieldnotes is what I actually needed. A single place where knowledge lives, organized so agents can find it, maintained so it stays true, and structured so it travels — between projects, between sessions, between collaborators, between AI tools.

Built because the problem was real, not because it seemed like a good idea in the abstract.

This specific problem is one instance of a general pattern. Humans and AI working together are not limited by capability — they're limited by coordination. The context that makes AI-assisted work valuable doesn't live where the next human-AI pair can use it. Every new session starts cold. Every new project starts from scratch. fieldnotes started as a homelab tool. The shape of the solution turned out to be general: making human-AI work reproducible, transferable, and auditable. The homelab is where I'm testing it; the framework is for anyone with the same problem.

---

## Why "fieldnotes"

Field notes are what researchers write when they're actually in the field — raw observations, half-formed ideas, things that need follow-up. They're not polished. They're not final. But they're real, they're dated, and they're the foundation that everything else gets built on.

That's the spirit here. Knowledge earned while doing something real, organized well enough to be useful later, maintained by agents who know what they're responsible for.

---

## How this is being built

fieldnotes is being developed using the same human-AI collaboration model it's designed to support.

The human sets direction and makes final decisions. The agent brings technical depth, research, structure, and execution. Every major decision in this repo — the two-repo model, the fieldguide execution protocol, the agent hierarchy — came from a conversation, not a solo design session.

**The process:**
1. Problem identified through real work — two AI-assisted projects with no way to share knowledge
2. Working-backwards README written first — the north star before any code
3. Requirements derived from the README, grounded in real projects
4. Design, then implementation, then validation against the original README

**The planning discipline:** before any non-trivial document or design is written, the structure is presented as bullet points and reviewed. The full content only gets written after the structure is approved. This keeps the work aligned and avoids large rewrites.

The commit history tells this story. Every commit message explains what changed, why, and what it enables — including which AI agent produced it.

**Key documents:**
- [`PHILOSOPHY.md`](PHILOSOPHY.md) — what fieldnotes believes about human-AI collaboration and why it looks the way it does
- [`TENETS.md`](TENETS.md) — the operational principles that govern decisions
- [`journal/`](journal/) — session-by-session record of how the project has evolved, including named collaboration patterns (The Redirect, The Bullet Check, The Perspective Check) that shape how we work
- [`.kiro/steering/fieldnotes-dev.md`](.kiro/steering/fieldnotes-dev.md) — the agent collaboration guide

**Continuing development in a new session:**

When opening this project with a new agent, start with:

> *"Read STATUS.md and the steering files first, then tell me where we are and what comes next."*

This triggers the session startup sequence defined in `fieldnotes-dev.md`. The agent reads the current phase, the last session summary, and the next items on the checklist — then confirms its understanding before doing anything. If the agent skips this and jumps straight to action, redirect it.

---

## How it works

### Two repos, not one

fieldnotes is a framework, not a knowledge base. It defines the schema, the agent protocol, and the rules. You create your own KB repo that follows those rules. Your knowledge is yours — public or private, on whatever topics you care about.

```
fieldnotes (this repo)           your-kb (your repo)
  schema + agent specs    →      your articles + your agents
  MCP server              →      your knowledge + your guides
  protocol definition     →      your execution sessions
```

When fieldnotes improves, you pull the update. Your content is untouched.

Because everyone uses the same schema, any fieldnotes-compatible agent can read any fieldnotes KB. Knowledge becomes interoperable without being centralized.

### Two layers

fieldnotes has two distinct layers that work together:

**Access layer — MCP server**
Exposes your KB as tools any MCP-compatible client can call. Read articles, search by tag, load a guide, advance through steps, submit feedback. Any LLM with MCP support can use it — no custom agent required.

**Behavior layer — agent steering files**
Encodes researcher judgment: when to explore vs execute, how to maintain the KB over time, how to escalate, how to propose new knowledge areas. The Kiro implementation ships with Alpha. Other implementations can be built against the protocol spec.

You can use either layer independently. The MCP server alone gives you a structured KB any tool can read. Add the steering files and you get the full researcher behavior on top.

### The agent hierarchy

Every KB has a **lead researcher** and one or more **SME researchers**.

The lead researcher is who you talk to. It knows the full index of your KB and operates in two modes:

**Explore mode** — when you need to understand something new. The lead doesn't rush to produce documents. It maps what you already know, identifies the gaps, asks questions, thinks out loud with you. The goal is to understand the problem before producing anything. When a topic is deep enough to warrant a specialist, the lead proposes creating a new SME agent — but only with your approval. No agent creates another agent without human sign-off.

**Doer mode** — when you need direct action. "Audit all volatile articles." "Update the Smartnode version across all articles." "Load the setup guide and continue from where we left off." The lead delegates to the right SME, tracks what changed, reports back.

SME researchers own their domain. They sign every article they create or modify. They run audits on their own scope. Growth is controlled: the lead proposes, the human approves.

### What fieldnotes stores

Four document types, each with a different lifecycle:

**Knowledge articles** — facts, decisions, rationale. Living documents, updated as the world changes. Every fact has a source and an access date. Every article has a staleness model.

**Fieldguides** — human-AI execution guides. Structured for both humans to read and agents to execute. Contains embedded execution protocol so any LLM can load and run it via MCP without custom training. Meant to be shared, composed, and improved through use.

**Reports** — point-in-time analyses. Security audits, research summaries, one-time findings. Never updated — superseded by new reports. Historical record.

**Sessions** — execution state for a fieldguide in progress. Tracks current phase, completed steps, decisions made, open issues. Lives alongside the fieldguide. When you open fieldnotes on a new machine, the agent reads the session and knows exactly where you are.

### The staleness model

Every article has a volatility level that determines its audit cycle:

| Volatility | Audit cycle | Examples |
|---|---|---|
| `stable` | 12 months | Hardware specs, architectural decisions |
| `slow` | 3 months | OS versions, client versions, config patterns |
| `volatile` | 2 weeks | URLs, version numbers, API endpoints |
| `ephemeral` | Never — snapshot only | Queue positions, prices, one-time observations |

Agents know which articles need attention. `volatile` articles can be auto-verified by fetching the source. `stable` articles get flagged for human review. `ephemeral` articles are never audited — they carry a point-in-time warning.

### What a knowledge article looks like

```markdown
---
id: hardware-beelink-gti15
project: rocketpool-node
type: knowledge
title: "Beelink GTI15 — Hardware Reference"
tags:
  domain: [hardware]
  volatility: stable
  status: verified
agent: rocketpool-researcher/v1.0
created: 2026-04-18
updated: 2026-04-18
audit_due: 2027-04-18
sources:
  - url: "https://www.bee-link.com/blogs/all"
    accessed: 2026-04-18
    note: "BIOS T205 confirmed current"
---

## Summary
## Facts
## Decisions & Rationale
## Known Issues
## Open Questions
## Changelog
```

→ [Full schema documentation](schema/article-format.md)

### What a fieldguide looks like

```markdown
---
id: fieldguide-rocketpool-node-setup
project: rocketpool-node
type: fieldguide
status: alpha
title: "Rocket Pool Node Setup — Ubuntu 24.04 + Saturn 1"
tags:
  domain: [guide, hardware, protocol, os]
  volatility: slow
kb_references:
  - hardware-beelink-gti15
  - protocol-rocketpool-saturn1
  - os-ubuntu-2404
execution_model:
  human_steps: true
  agent_steps: true
  requires_approval: true
---

## Purpose
## Prerequisites
## Execution Summary

## Phases
<!-- Each step tagged with execution type, completion condition, feedback hooks -->

## Changelog
```

→ [Full fieldguide schema](schema/fieldguide-format.md)

---

## The fieldguide execution protocol

This is the idea at the heart of fieldnotes.

A fieldguide contains not just content but execution protocol — what to do, how to advance, how to report back. Any LLM that can call MCP tools can execute it. No custom agent. No specialized training. The guide drives the behavior.

Each step in a fieldguide declares:

```yaml
- id: phase-3-step-2
  title: "Configure UFW firewall"
  type: agent_executable        # human_required | agent_executable | approval_gate | verification
  completion:
    command: "sudo ufw status | grep -q 'Status: active' && echo PASS || echo FAIL"
    expected: "PASS"
  advance_when: verification_passes
  feedback:
    types: [correction, timing, alternative_approach, issue]
```

The execution loop for any LLM:

```
1. fieldguide_load(id)          → guide content + current session state + next step
2. fieldguide_get_context(id)   → referenced KB articles loaded as context
3. Read next pending step
4. Execute based on type:
   human_required  → present instructions, wait for confirmation
   agent_executable → run, verify, report
   approval_gate   → present plan, wait for explicit yes
   verification    → run check, report PASS/FAIL
5. fieldguide_advance(id, step_id, result)
6. If feedback → fieldguide_submit_feedback(...)
7. Repeat
```

The guide gets better with every execution. Feedback from real runs — timing, errors, alternative approaches, environment-specific issues — goes back to the maintainer. The maintainer updates the guide. Everyone who runs it next benefits.

---

## Quick start

**Prerequisites:** Git, Python 3.11+, an MCP-compatible client (Kiro, Claude Desktop, Cursor, or any MCP client)

**1. Get the framework**
```bash
git clone https://github.com/[org]/fieldnotes
```

**2. Create your KB repo**
```bash
mkdir my-kb && cd my-kb
git init
cp ../fieldnotes/examples/minimal-kb/fieldnotes.yml .
# Edit fieldnotes.yml — set your project name and fieldnotes version
```

**3. Start the MCP server**
```bash
uvx fieldnotes-mcp --kb-path ./my-kb
```

**4. Connect your client**

For Kiro: copy `fieldnotes/implementations/kiro/` to your project's `.kiro/` folder.
For other MCP clients: add the server to your MCP config — see [implementations/](implementations/).

**5. Talk to the lead researcher**

Open a new session and say: *"I want to start building a KB for [your topic]."*
The lead researcher takes it from there.

→ [Full setup guide](implementations/kiro/README.md)

---

## MCP tools reference

The fieldnotes MCP server exposes these tools to any connected client:

**KB operations**
- `kb_search(tags, project?)` — find articles by tag
- `kb_get(article_id)` — get a full article
- `kb_create(article)` — create a new article (validates against schema)
- `kb_update(article_id, changes, changelog_entry)` — update article, append changelog
- `kb_audit(project?, tags?)` — list articles where audit_due < today

**Fieldguide operations**
- `fieldguide_load(id)` — load guide + session state + next pending step
- `fieldguide_get_context(id)` — load all referenced KB articles
- `fieldguide_advance(id, step_id, result)` — mark step complete, get next step
- `fieldguide_submit_feedback(id, step_id, type, content)` — submit feedback to maintainer

**Agent operations**
- `agent_propose(spec)` — propose a new SME agent (lead researcher only)

→ [Full protocol spec](schema/agent-protocol.md)

---

## Implementations

**MCP server** — available in Alpha. Python, installable via `uvx`. Runs locally, reads your KB folder directly. → [mcp-server/](mcp-server/)

**Kiro** — available in Alpha. Steering files for lead researcher + SME template. Full explore and doer mode behavior. → [implementations/kiro/](implementations/kiro/)

**Other clients** — the protocol is open. Build your own implementation against [schema/agent-protocol.md](schema/agent-protocol.md). Contributions welcome.

---

## Alpha — what works, what doesn't

**What's in Alpha:**
- Article schema with `modified_by` provenance history (knowledge, fieldguide, report, session)
- Tag taxonomy with tagging guidelines for agents
- Fieldguide execution protocol — step types, completion conditions, feedback hooks
- Fieldguide Quick Summary block (outcome, starting/ending state, time, difficulty, reversibility)
- Optional step-level `tip`, `warning`, and `detailed_explanation` fields
- Agent Autonomy Rule and explicit Handoff Protocol between step types
- Fieldguide composition via `depends_on_fieldguides`
- Improvement backlog generated from execution feedback
- MCP server with all KB and fieldguide operations
- Lead researcher spec + Kiro steering file
- SME researcher template + Kiro steering file
- Session documents — portable execution state
- Minimal example KB with one knowledge article and one fieldguide

**What's not built yet:**
- Autonomous remediation — authors can declare `remediation` steps, but Alpha's default response to failure is to report and wait
- Automated audit runner — audit logic exists, no cron/hook to trigger it automatically
- Multi-KB discovery — no way to search across multiple KB repos
- Agent-to-agent handoff protocol — SMEs can't formally delegate to each other yet
- Feedback review UI — feedback is collected, review is manual
- Schema conformance validator — additive-only schema means no breaking changes, but no CLI to validate existing articles
- Web UI — everything is files, MCP tools, and agents

**Known limitations:**
- The MCP server runs locally only. No hosted option.
- Article signing is by convention, not enforced cryptographically.
- The Kiro implementation is the only behavior layer available. Other clients get MCP tools but not explore/doer mode behavior until they build their own implementation.
- Session documents are public if your KB is public. This is intentional but worth knowing.

This is a working-in-progress. I'm building it because I need it. The first real KB using this framework will be a homelab KB covering infrastructure, Ethereum staking, and whatever comes next — planned, not yet created. That project is where Alpha gets validated against real use.

---

## Contributing

**Improve the framework** — better schema, better agent specs, new MCP tools, new implementations. Open a PR. Changes to `schema/` need a clear rationale — the schema is the contract everything else depends on.

**Publish your own KB** — create a KB repo, follow the schema, make it public. That's a contribution to the ecosystem even if you never touch this repo.

**Submit fieldguide feedback** — if you run a fieldguide and something is wrong, use `fieldguide_submit_feedback`. That's the mechanism. Use it.

What belongs here vs in your KB: if it's about how the system works, it belongs here. If it's knowledge about a topic, it belongs in a KB repo.

→ [CONTRIBUTING.md](CONTRIBUTING.md)

---

## License

MIT — use it, fork it, build on it.

---

*fieldnotes grew out of a real problem encountered while building a complex technical project with an AI collaborator. The first public KB built on this framework — a homelab covering infrastructure, Ethereum staking, and more — is planned and will be released alongside Alpha as a reference implementation.*
