# Article Format Schema

> fieldnotes schema v1.0 — Alpha

This document defines the canonical structure for all articles in a fieldnotes knowledge base. Every article — knowledge, fieldguide, report, or session — must conform to this schema to be valid.

**Audience:** Agents writing or validating articles, humans authoring articles by hand, and the MCP server's parser/serializer.

---

## Frontmatter Fields

Every article begins with a YAML frontmatter block delimited by `---`. Fields must appear in the canonical order defined below.

### Universal Fields (all article types)

| Field | Type | Required | Description |
|---|---|---|---|
| `id` | string | Yes | Unique identifier. Kebab-case, descriptive. Example: `hardware-beelink-gti15` |
| `type` | enum | Yes | One of: `knowledge`, `fieldguide`, `report`, `session` |
| `title` | string | Yes | Human-readable title. Quoted if it contains special characters. |
| `project` | string | Yes | Primary project this article belongs to. Example: `rocketpool-node` |
| `related_projects` | list of strings | No | Additional projects this article is relevant to. |
| `tags` | block | Yes | Structured metadata block. See [Tags Block](#tags-block). |
| `agent` | string | Yes | Name and version of the agent that last modified this article. Format: `agent-name/vX.Y` |
| `modified_by` | list | No | Append-only provenance history. See [Modified By](#modified-by). |
| `created` | date | Yes | Date the article was first created. Format: `YYYY-MM-DD` |
| `updated` | date | Yes | Date the article was last modified. Format: `YYYY-MM-DD` |

### Tags Block

The `tags` block is a required nested structure within the frontmatter.

| Field | Type | Required | Description |
|---|---|---|---|
| `domain` | list of strings | Yes | Knowledge domains. Examples: `hardware`, `protocol`, `os`, `network`, `guide` |
| `volatility` | enum | Yes | One of: `stable`, `slow`, `volatile`, `ephemeral`. See [audit-rules.md](audit-rules.md). |
| `status` | enum | Yes | One of: `draft`, `verified`, `needs-review` |
| `physical_id` | string | No | Physical device identifier. Required by convention for hardware articles. Example: `beelink-gti15-001` |

### Modified By

Optional, append-only list tracking every agent that created or modified the article. The MCP server appends to this list automatically on `kb_create` and `kb_update`.

Each entry:

| Field | Type | Description |
|---|---|---|
| `agent` | string | Agent name and version. Format: `agent-name/vX.Y` |
| `date` | date | Date of the action. Format: `YYYY-MM-DD` |
| `action` | enum | One of: `created`, `updated` |

### Knowledge-Specific Fields

These fields are required when `type: knowledge`.

| Field | Type | Required | Description |
|---|---|---|---|
| `audit_due` | date | Yes (unless `volatility: ephemeral`) | Next audit date. Computed as `updated` + volatility audit cycle. See [audit-rules.md](audit-rules.md). |
| `sources` | list | Yes | Sources backing the article's facts. Each entry: see below. |

Each `sources` entry:

| Field | Type | Required | Description |
|---|---|---|---|
| `url` | string | Yes | Source URL. |
| `accessed` | date | Yes | Date the source was last accessed. Format: `YYYY-MM-DD` |
| `note` | string | No | Brief description of what was verified from this source. |

### Session-Specific Fields

These fields are required when `type: session`.

| Field | Type | Required | Description |
|---|---|---|---|
| `fieldguide_id` | string | Yes | The `id` of the fieldguide this session tracks. |
| `status` | enum | Yes | One of: `in_progress`, `complete`, `abandoned` |
| `started` | date | Yes | Date the session began. Format: `YYYY-MM-DD` |

### Fieldguide-Specific Fields

These fields are required when `type: fieldguide`. Full fieldguide structure is defined in [fieldguide-format.md](fieldguide-format.md).

| Field | Type | Required | Description |
|---|---|---|---|
| `kb_references` | list of strings | Yes | Article IDs that provide context for executing this guide. |
| `depends_on_fieldguides` | list of strings | No | Fieldguide IDs that must be complete before this guide can start. |
| `execution_model` | block | Yes | Declares execution characteristics. See below. |

`execution_model` block:

| Field | Type | Required | Description |
|---|---|---|---|
| `human_steps` | boolean | Yes | Whether the guide contains `human_required` steps. |
| `agent_steps` | boolean | Yes | Whether the guide contains `agent_executable` steps. |
| `requires_approval` | boolean | Yes | Whether the guide contains `approval_gate` steps. |

---

## Required Sections by Type

Every article body (below the frontmatter) must include specific markdown sections in the order listed. The Changelog section is always last.

### Knowledge Articles

```
## Summary
## Facts
## Decisions & Rationale
## Known Issues
## Open Questions
## Changelog
```

### Fieldguide Articles

Defined in [fieldguide-format.md](fieldguide-format.md). At minimum:

```
## Summary
## Changelog
```

### Report Articles

```
## Summary
## Findings
## Changelog
```

### Session Articles

```
## Summary
## Completed Steps
## Decisions
## Open Issues
## Changelog
```

---

## Canonical Field Ordering

The serializer must produce frontmatter fields in this exact order. This ensures articles are human-readable and diff-friendly across edits.

```yaml
id:
type:
title:
project:
related_projects:       # optional
tags:
  domain:
  volatility:
  status:
  physical_id:          # optional
agent:
modified_by:            # optional
created:
updated:
audit_due:              # knowledge only, unless ephemeral
sources:                # knowledge only
fieldguide_id:          # session only
status:                 # session only (session status, distinct from tags.status)
started:                # session only
kb_references:          # fieldguide only
depends_on_fieldguides: # fieldguide only, optional
execution_model:        # fieldguide only
  human_steps:
  agent_steps:
  requires_approval:
```

Fields not applicable to the article type are omitted entirely — they are not included with empty values.

---

## Examples

### Knowledge Article

```yaml
---
id: hardware-beelink-gti15
type: knowledge
title: "Beelink GTI15 — Hardware Reference"
project: rocketpool-node
related_projects:
  - homelab-network
tags:
  domain: [hardware]
  volatility: stable
  status: verified
  physical_id: beelink-gti15-001
agent: rocketpool-researcher/v1.0
modified_by:
  - agent: rocketpool-researcher/v1.0
    date: 2026-04-18
    action: created
created: 2026-04-18
updated: 2026-04-18
audit_due: 2027-04-18
sources:
  - url: "https://www.bee-link.com/blogs/all"
    accessed: 2026-04-18
    note: "BIOS T205 confirmed current"
---
```

### Report Article

```yaml
---
id: report-kb-audit-2026-06
type: report
title: "KB Audit Report — June 2026"
project: rocketpool-node
tags:
  domain: [audit]
  volatility: ephemeral
  status: verified
agent: lead-researcher/v1.0
modified_by:
  - agent: lead-researcher/v1.0
    date: 2026-06-15
    action: created
created: 2026-06-15
updated: 2026-06-15
---
```

### Session Article

```yaml
---
id: session-rocketpool-setup-001
type: session
title: "Rocket Pool Node Setup — Execution Session 1"
project: rocketpool-node
tags:
  domain: [guide, hardware, protocol]
  volatility: ephemeral
  status: draft
agent: lead-researcher/v1.0
modified_by:
  - agent: lead-researcher/v1.0
    date: 2026-05-01
    action: created
  - agent: lead-researcher/v1.0
    date: 2026-05-03
    action: updated
created: 2026-05-01
updated: 2026-05-03
fieldguide_id: fieldguide-rocketpool-node-setup
status: in_progress
started: 2026-05-01
---
```

### Fieldguide Article (frontmatter only — full structure in fieldguide-format.md)

```yaml
---
id: fieldguide-rocketpool-node-setup
type: fieldguide
title: "Rocket Pool Node Setup — Ubuntu 24.04 + Saturn 1"
project: rocketpool-node
tags:
  domain: [guide, hardware, protocol, os]
  volatility: slow
  status: draft
agent: rocketpool-researcher/v1.0
modified_by:
  - agent: rocketpool-researcher/v1.0
    date: 2026-04-20
    action: created
created: 2026-04-20
updated: 2026-04-20
kb_references:
  - hardware-beelink-gti15
  - protocol-rocketpool-saturn1
  - os-ubuntu-2404
execution_model:
  human_steps: true
  agent_steps: true
  requires_approval: true
---
```

---

## Additive-Only Rule

The schema is additive-only. This means:

- New fields may be added in any schema version. New fields are always optional.
- No existing field may be removed, renamed, or have its type changed.
- No existing enum value may be removed from any enum field.
- An article valid under schema v1.0 must remain valid under all v1.x releases.

When adding a new field, document it in this file with its type, whether it's optional, and which article types it applies to. Update the canonical field ordering to show where it appears.
