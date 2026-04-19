# Contributing to fieldnotes

fieldnotes is an open project. There are two ways to contribute.

## 1. Improve the framework

The framework is the schema, the agent protocol, the MCP server, and the agent steering files. If you find a bug, have a better approach, or want to add a new implementation — open a PR.

**Rules for schema changes:**
- The schema is additive-only. New fields must always be optional.
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

## 2. Publish your own KB

Create a KB repo, follow the schema, make it public. That's a contribution to the ecosystem even if you never touch this repo.

Convention for KB repo names: `fieldnotes-kb-[topic]`

If you want your KB listed in the registry (coming in a future release), open an issue with a link to your repo.

## What belongs here vs in your KB

If it's about how the system works → it belongs here.
If it's knowledge about a topic → it belongs in a KB repo.

## Getting started

```bash
git clone https://github.com/yzumbado/fieldnotes
cd fieldnotes
# MCP server setup instructions in mcp-server/README.md
```
