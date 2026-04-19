# Tag Taxonomy

> fieldnotes schema v1.0 — Alpha

This document defines the tag system used across all fieldnotes articles. Tags are the primary mechanism for search, classification, and audit filtering. They live in the `tags` block of every article's frontmatter.

**Audience:** Agents creating or updating articles, humans authoring articles, and the MCP server's `kb_search` and `kb_audit` tools.

**Related:** [article-format.md](article-format.md) defines where tags appear in the frontmatter. [audit-rules.md](audit-rules.md) defines how volatility drives audit behavior.

---

## Tag Structure

Tags are not flat strings. They live in a structured `tags` block within the YAML frontmatter:

```yaml
tags:
  domain: [hardware, network]
  volatility: stable
  status: verified
  physical_id: beelink-gti15-001    # optional
```

The `tags` block is required on every article. Individual fields within it may be required or optional as documented below.

---

## Domain Tags

**Type:** List of strings. Required. At least one domain tag must be present.

Domain tags describe what the article is about. They are an open vocabulary — KB owners can add new domains freely. The schema does not restrict the list, but provides recommended starting values and naming conventions.

### Recommended Domains

| Domain | Use for |
|---|---|
| `hardware` | Physical devices, specs, BIOS, firmware |
| `protocol` | Blockchain protocols, network protocols, communication standards |
| `os` | Operating system configuration, kernel, packages |
| `network` | Networking config, VLANs, firewall, DNS, routing |
| `security` | Security posture, certificates, access control, hardening |
| `config` | Application configuration, Docker, service setup |
| `guide` | Fieldguides and execution-oriented content |
| `audit` | Audit reports and findings |
| `containers` | Docker, container orchestration, images |
| `storage` | Disk, filesystem, NAS, backup |
| `monitoring` | Metrics, alerting, logging, observability |

### Naming Convention

- Lowercase, singular form: `hardware` not `Hardware` or `hardwares`
- Kebab-case for multi-word domains: `home-lab` not `homelab` or `home_lab`
- Descriptive but concise: prefer `network` over `networking-and-connectivity`

---

## Volatility Tags

**Type:** Enum. Required. Exactly one value.

Volatility declares how quickly an article's facts go stale. It determines the audit cycle — see [audit-rules.md](audit-rules.md) for full audit behavior.

| Value | Audit Cycle | Description |
|---|---|---|
| `stable` | 12 months | Facts that rarely change. |
| `slow` | 3 months | Facts tied to software release cycles. |
| `volatile` | 2 weeks | Facts that change frequently. |
| `ephemeral` | Never | Point-in-time snapshots. Never audited. |

### Classification Guidelines

| Content Type | Volatility | Reasoning |
|---|---|---|
| Hardware specifications | `stable` | Physical specs don't change after purchase |
| Architectural decisions | `stable` | Decisions are stable once made — rationale doesn't expire |
| BIOS/firmware versions | `slow` | Updates happen but infrequently |
| OS versions, kernel versions | `slow` | Tied to release cycles (months) |
| Client software versions | `slow` | Tied to release cycles |
| Configuration patterns | `slow` | Change when software updates require it |
| Docker image tags | `volatile` | New tags published frequently |
| URLs, API endpoints | `volatile` | Can change or break at any time |
| Version numbers in commands | `volatile` | Outdated quickly |
| Queue positions, gas prices | `ephemeral` | Valid only at the moment of observation |
| Market prices, exchange rates | `ephemeral` | Valid only at the moment of observation |
| One-time observations | `ephemeral` | Not expected to be re-verified |

### Volatility Decision Tree

When unsure which volatility to assign:

1. Is this a one-time observation or snapshot? → `ephemeral`
2. Does this change with URLs, version numbers, or external APIs? → `volatile`
3. Does this change with software releases (months)? → `slow`
4. Is this about physical hardware or a decision that was made? → `stable`
5. Still unsure? → Default to `slow` and revisit at first audit.

---

## Status Tags

**Type:** Enum. Required. Exactly one value.

Status tracks the verification lifecycle of an article.

| Value | Meaning | Set by |
|---|---|---|
| `draft` | Newly created. Facts have not been independently verified. | Agent on `kb_create` |
| `verified` | Facts confirmed against sources. Sources are current. | Human or agent after verification |
| `needs-review` | Flagged for re-verification. May be triggered by audit or manual flag. | Audit process or human |

### Status Transitions

```
draft → verified       (after human or agent verifies facts against sources)
verified → needs-review (when audit_due passes, or when a human flags it)
needs-review → verified (after re-verification confirms facts are still current)
needs-review → draft    (if re-verification reveals significant changes needed)
```

An article should never move backward from `verified` to `draft` unless the content has changed substantially enough that it's effectively a new article.

---

## Physical ID Tag

**Type:** String. Optional. Required by convention for hardware articles.

Identifies a specific physical device so that agents working on different projects can find all articles about the same machine.

### Naming Convention

Format: `device-model-instance`

Examples:
- `beelink-gti15-001` — first Beelink GTI15 in the lab
- `rpi4-002` — second Raspberry Pi 4
- `unifi-udm-pro-001` — UniFi Dream Machine Pro

The instance number distinguishes multiple devices of the same model. If you only have one, use `001`.

---

## Project and Related Projects

`project` and `related_projects` are frontmatter fields outside the `tags` block, but they interact with tag-based search.

- `project` — required on every article. Identifies the primary project.
- `related_projects` — optional list. Additional projects this article is relevant to.

### Interaction with Search

`kb_search` accepts an optional `project` filter:
- When `project` is provided: returns articles where `project` matches OR the project appears in `related_projects`.
- When `project` is omitted: searches across all projects.

Tag filters and project filters combine with AND logic: an article must match both the tag filter and the project filter to be returned.

---

## Tagging Guidelines

These guidelines help agents and humans make consistent tagging decisions.

### Specificity Rule

Tag for what the article is *about*, not what it *mentions*.

An article about UFW firewall configuration that happens to mention Docker should be tagged `network`, `security` — not `network`, `security`, `containers`. The Docker mention is incidental context, not the subject.

Ask: "If someone searched for this tag, would they expect to find this article?" If the answer is "only tangentially," don't add the tag.

### Domain Tag Count

Aim for 1–3 domain tags per article. If you need more than 3, the article may be covering too many topics and should be considered for splitting into separate articles.

- 1 tag: focused article on a single topic (most common)
- 2 tags: article at the intersection of two domains (e.g., `hardware` + `network` for a NIC configuration article)
- 3 tags: broad article covering a multi-domain topic (e.g., a fieldguide touching hardware, OS, and protocol)
- 4+ tags: reconsider scope — is this article trying to do too much?

### Volatility Selection

Use the [decision tree](#volatility-decision-tree) above. When an article contains facts at different volatility levels (e.g., stable hardware specs and volatile version numbers), tag with the *most volatile* level. The article will be audited at the shorter cycle, which catches the volatile facts. During audit, the stable facts can be quickly confirmed.

### Status on Creation

Agents always set `status: draft` when creating a new article via `kb_create`. Only a human review or a verification process (agent checks facts against sources) should promote to `verified`.

### Cross-Project Tagging

If an article is relevant to multiple projects, use `related_projects` in the frontmatter — do not add project names as domain tags. Domain tags describe *what the article is about*. Project fields describe *which projects use it*.

```yaml
# Correct
project: rocketpool-node
related_projects:
  - homelab-network
tags:
  domain: [hardware]

# Incorrect — project names as domain tags
tags:
  domain: [hardware, rocketpool, homelab]
```

### Consistency Check

Before creating a new domain tag, search existing articles to see if a similar tag already exists:
- Prefer `network` over creating `networking`
- Prefer `containers` over creating `docker` (unless Docker-specific content warrants it)
- Prefer `os` over creating `linux` or `ubuntu` (unless the article is specifically about a distro, not OS config in general)

When in doubt, use the broader tag. Narrower tags can be added later if the KB grows enough to warrant them.

### Tagging Checklist for Agents

When creating or updating an article, verify:

1. At least one domain tag is present
2. Domain tags describe the article's subject, not incidental mentions
3. No more than 3 domain tags (or justify why more are needed)
4. Volatility matches the most volatile fact in the article
5. Status is `draft` for new articles
6. `physical_id` is set if the article is about specific hardware
7. No project names appear in domain tags — use `project`/`related_projects` instead
8. Domain tag doesn't duplicate an existing tag with a different name

---

## Search Behavior

The `kb_search` tool uses tags as follows:

- **Domain filter:** When multiple domain tags are provided in a search, articles matching *any* of the provided domains are returned (OR logic). This surfaces related content across domains.
- **Project filter:** Combined with tag filters using AND logic. An article must match the project filter AND the tag filter.
- **Volatility/status filter:** When provided, matches exactly (AND with other filters).

Example: `kb_search(tags: {domain: ["hardware", "network"]}, project: "rocketpool-node")` returns articles in the `rocketpool-node` project (or with it in `related_projects`) that have `hardware` OR `network` in their domain tags.

---

## Extending the Taxonomy

The tag taxonomy follows the additive-only rule:

- New tag fields may be added to the `tags` block in future schema versions. New fields are always optional.
- No existing tag field may be removed or renamed.
- No existing enum value may be removed from `volatility` or `status`.
- New enum values may be added to `volatility` or `status` in future versions.
- New recommended domain tags may be added to the list above. Existing recommendations are never removed.

When adding a new tag field, document it in this file with its type, whether it's optional, and its purpose. Update [article-format.md](article-format.md) to include it in the tags block definition and canonical field ordering.
