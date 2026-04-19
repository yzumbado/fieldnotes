# fieldnotes — Specs

This folder contains the design and requirements documents for the fieldnotes framework.
These documents capture the full evolution of the project from initial concept to implementation.

## Documents

| File | Description | Status |
|---|---|---|
| `requirements.md` | Alpha requirements — 16 requirements with acceptance criteria | Complete |
| `design.md` | Technical design — architecture, MCP server, schema | In progress |
| `tasks.md` | Implementation task list | Pending |

## Process

fieldnotes follows a requirements-first spec workflow:
1. Requirements → what the system must do
2. Design → how it will do it
3. Tasks → what to build and in what order

The requirements document was written working backwards from the Alpha README,
grounded in two real projects that surfaced the general problem fieldnotes solves:
- A Rocket Pool Ethereum staking node setup guide
- A home lab network configuration project

Both projects shared physical hardware but had no way to share knowledge between them.
That's the specific problem that surfaced the general coordination gap — it generalizes to any repeatable,
partially-automatable human-AI collaboration where context doesn't travel between sessions,
projects, or tools.
