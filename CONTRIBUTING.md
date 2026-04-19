# Contributing to fieldnotes

fieldnotes is an open project. There are three ways to contribute.

Before contributing, read [TENETS.md](TENETS.md) — the operational principles that govern decisions. PRs that conflict with the tenets will be asked to justify the deviation or revise.

## 1. Improve the framework

The framework is the schema, the agent protocol, the MCP server, and the agent steering files. If you find a bug, have a better approach, or want to add a new implementation — open a PR.

**Rules for schema changes:**
- The schema is additive-only (Tenet 9). New fields must always be optional.
- No field may be removed or renamed in a way that invalidates existing articles.
- Every schema change needs a clear rationale in the PR description.
- If your change requires existing KB repos to update their articles, it's a breaking change and will not be accepted.

**Rules for MCP server changes:**
- All existing tools must remain backward compatible.
- New tools are welcome. Removing or renaming tools is a breaking change.
- Tests required for any new tool.

**Rules for agent steering files:**
- Changes to the lead researcher or SME template affect every KB that uses them.
- Test against the minimal example KB before submitting.

## 2. Publish reusable fieldguides

Fieldguides are the social artifact of this framework — they're meant to be shared, composed, and improved through use. If you've built a workflow that reliably produces a digital output with AI collaboration, packaging it as a fieldguide is a contribution to the ecosystem even if you never touch this repo.

**What makes a good shareable fieldguide:**
- It produces a specific, reproducible output (a configured server, an edited video, a completed audit)
- Steps declare their execution type (`agent_executable` vs `human_required` vs `approval_gate` vs `verification`)
- Completion conditions are verifiable (command output, not vibes)
- `kb_references` point to knowledge articles that provide context
- It has been executed at least once and its feedback incorporated

Publish your fieldguides in your own KB repo, or contribute them to a shared registry (coming in a future release). Either way, the schema ensures portability — a good fieldguide works on any MCP-compatible LLM.

## 3. Publish your own KB

Create a KB repo, follow the schema, make it public. That's a contribution to the ecosystem even if you never touch this repo.

Convention for KB repo names: `fieldnotes-kb-[topic]`

If you want your KB listed in the registry (coming in a future release), open an issue with a link to your repo.

## What belongs here vs in your KB

If it's about how the system works → it belongs here.
If it's knowledge about a topic, or a fieldguide for producing something → it belongs in a KB repo.

## Reporting fieldguide issues

If you run a fieldguide and something goes wrong, use `fieldguide_submit_feedback` to report it. That's the mechanism — it's structured, traceable, and feeds the backlog the guide maintainer uses to improve the guide. Generic GitHub issues for fieldguide problems will be closed with a pointer to the feedback mechanism.

## Getting started

```bash
git clone https://github.com/yzumbado/fieldnotes
cd fieldnotes
# MCP server setup instructions in mcp-server/README.md
```
