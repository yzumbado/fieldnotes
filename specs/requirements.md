# Requirements Document — fieldnotes

## Introduction

fieldnotes is a framework for making human-AI work reproducible, transferable, and auditable. It defines a schema, an agent protocol, and an MCP server that any compatible LLM can use to read, write, and execute against a structured knowledge base.

Humans and AI working together are not limited by capability — they are limited by coordination. Most AI collaboration today is ephemeral: it lives in one conversation, on one machine, with one pair of participants. When the session ends, the context that made the work valuable disappears. The next session starts cold. The next person starts from scratch. Work that could compound across time, machines, and collaborators instead evaporates.

The concrete problem that surfaced this gap: two projects sharing the same physical hardware (a Beelink GTI15) accumulated decisions in isolation. The Rocket Pool project knew the machine as a staking node; the home lab network project knew it as a network device. Neither project could see what the other had decided. When a cross-cutting change was needed, there was no mechanism to carry knowledge across the boundary.

fieldnotes solves the general problem through a specific mechanism: giving knowledge a home — a structured, agent-navigable repository — and giving procedures an executable protocol — fieldguides that any MCP-compatible LLM can run. The homelab was the first instance; the framework generalizes to any repeatable, partially-automatable work where human judgment is the bridge and AI can absorb the grind.

**Alpha scope:** Schema files, MCP server (KB + fieldguide operations), lead researcher spec + Kiro steering file, SME researcher template + Kiro steering file, session document type, minimal example KB, and the fieldguide execution protocol — including `modified_by` provenance, fieldguide composition via `depends_on_fieldguides`, the Quick Summary block, optional `tip`/`warning`/`detailed_explanation` step fields, the Agent Autonomy Rule and Handoff Protocol, and the improvement backlog generated from execution feedback.

For the project's belief system, see [PHILOSOPHY.md](../PHILOSOPHY.md). For operational principles, see [TENETS.md](../TENETS.md).

---

## Glossary

- **KB**: Knowledge Base — a user-owned repository of articles following the fieldnotes schema.
- **Framework_Repo**: The `fieldnotes` repository — defines schema, agent protocol, and MCP server. Contains no user knowledge.
- **KB_Repo**: A user-owned repository that follows the fieldnotes schema and contains actual knowledge articles.
- **MCP_Server**: The fieldnotes MCP server — exposes KB operations as tools to any MCP-compatible LLM client.
- **Lead_Researcher**: The top-level agent in a KB. Entry point for all user interactions. Operates in explore mode or doer mode.
- **SME_Researcher**: A subject-matter-expert agent that owns a specific knowledge domain within a KB.
- **Article**: A single document in a KB. One of four types: `knowledge`, `fieldguide`, `report`, or `session`.
- **Knowledge_Article**: A living document containing facts, decisions, and rationale about a topic.
- **Fieldguide**: A structured human-AI execution guide with embedded execution protocol.
- **Report**: A point-in-time analysis document. Never updated — superseded by new reports.
- **Session**: An execution state document tracking progress through a fieldguide in progress.
- **Step**: A single unit of work within a fieldguide phase. Declares execution type, completion condition, and feedback hooks.
- **Staleness_Model**: The volatility classification of a knowledge article that determines its audit cycle.
- **Explore_Mode**: Lead researcher operating mode for collaborative discovery — maps gaps, asks questions, thinks out loud before producing output.
- **Doer_Mode**: Lead researcher operating mode for direct execution — audits, updates, migrations, guide execution.
- **Approval_Gate**: A fieldguide step type requiring explicit human confirmation before the agent proceeds.
- **Schema**: The YAML frontmatter and section structure that all fieldnotes articles must conform to.
- **Frontmatter**: YAML metadata block at the top of every article, delimited by `---`.
- **Changelog**: An append-only section at the bottom of every knowledge article recording what changed and when.
- **Feedback**: Structured observations submitted during fieldguide execution — corrections, timing notes, alternative approaches, issues encountered.
- **Audit**: The process of reviewing a knowledge article to verify its facts are still current.
- **Tag**: A structured metadata field in article frontmatter used for search and classification.
- **Modified_By**: An optional append-only list in article frontmatter recording every agent that created or modified the article, with date and action.
- **Backlog**: A structured, append-only file co-located with a fieldguide that tracks improvement items derived from execution feedback. Human-triaged in Alpha.

---

## Requirements

### Requirement 1: Two-Repo Separation

**User Story:** As a knowledge base owner, I want the framework code and my knowledge content to live in separate repositories, so that I can update the framework without touching my content and keep my knowledge private if I choose.

#### Acceptance Criteria

1. THE Framework_Repo SHALL contain schema definitions, agent protocol specifications, MCP server code, and agent steering files, and SHALL contain no user knowledge articles.
2. THE KB_Repo SHALL contain only user-authored articles, a `fieldnotes.yml` configuration file, and no framework code.
3. WHEN a new version of the Framework_Repo is released, THE KB_Repo SHALL require no changes to existing articles to remain compatible, provided the schema change is additive.
4. THE `fieldnotes.yml` configuration file SHALL declare the fieldnotes schema version the KB was created against, the KB name, and the default project name.
5. THE Framework_Repo SHALL provide a minimal example KB that a user can copy to bootstrap a new KB_Repo without writing any schema by hand.

---

### Requirement 2: Article Schema

**User Story:** As an agent or human reader, I want every article in the KB to follow a consistent, machine-parseable structure, so that I can reliably find, read, and update knowledge without custom parsing logic per article.

#### Acceptance Criteria

1. THE Schema SHALL require every article to include a YAML frontmatter block containing: `id`, `type`, `title`, `tags`, `agent`, `created`, and `updated` fields.
2. THE Schema SHALL require the `type` field to be one of: `knowledge`, `fieldguide`, `report`, or `session`.
3. THE Schema SHALL require the `tags` block to include a `volatility` field set to one of: `stable`, `slow`, `volatile`, or `ephemeral`.
4. THE Schema SHALL require the `tags` block to include a `status` field set to one of: `draft`, `verified`, or `needs-review`.
5. WHEN the article type is `knowledge`, THE Schema SHALL require an `audit_due` field computed from `updated` and the volatility audit cycle.
6. WHEN the article type is `knowledge`, THE Schema SHALL require a `sources` list where each entry includes a `url` and `accessed` date.
7. THE Schema SHALL require every article to include the following sections in order: Summary, and a Changelog section as the final section.
8. WHEN the article type is `knowledge`, THE Schema SHALL require Facts, Decisions & Rationale, Known Issues, and Open Questions sections between Summary and Changelog.
9. THE Schema SHALL be additive-only — new fields SHALL always be optional, and no field SHALL be removed or renamed in a way that invalidates existing articles.
10. THE Schema SHALL require the `agent` field to record the name and version of the agent that last modified the article, in the format `agent-name/vX.Y`.
11. THE Schema SHALL allow an optional `modified_by` list in the frontmatter, where each entry contains an `agent` name/version, a `date`, and an `action` (one of: `created`, `updated`). THE list SHALL be append-only — entries SHALL NOT be removed or reordered.
12. WHEN the `modified_by` field is present, THE MCP_Server SHALL append a new entry to it on every `kb_create` or `kb_update` operation, recording the acting agent, the current date, and the action performed.

---

### Requirement 3: Staleness Model

**User Story:** As a knowledge base maintainer, I want every article to declare how quickly its facts go stale, so that agents know which articles need attention and which can be trusted without re-verification.

#### Acceptance Criteria

1. THE Staleness_Model SHALL define four volatility levels with the following audit cycles: `stable` — 12 months, `slow` — 3 months, `volatile` — 2 weeks, `ephemeral` — never audited.
2. WHEN an article's volatility is `stable`, `slow`, or `volatile`, THE Schema SHALL require an `audit_due` date field set to `updated` plus the volatility audit cycle.
3. WHEN an article's volatility is `ephemeral`, THE Schema SHALL not require an `audit_due` field, and THE article SHALL carry a point-in-time warning in its Summary section.
4. WHEN the MCP_Server receives a `kb_audit` request, THE MCP_Server SHALL return all articles where `audit_due` is earlier than the current date, ordered by `audit_due` ascending.
5. THE Staleness_Model SHALL classify hardware specifications and architectural decisions as `stable`, OS and client versions as `slow`, URLs and version numbers as `volatile`, and queue positions and prices as `ephemeral`.

---

### Requirement 4: MCP Server — KB Operations

**User Story:** As an LLM agent using any MCP-compatible client, I want to read, search, create, and update knowledge articles through a standard tool interface, so that I can maintain the KB without direct file system access or custom integration code.

#### Acceptance Criteria

1. THE MCP_Server SHALL expose a `kb_search` tool that accepts `tags` and an optional `project` parameter and returns all matching article IDs and titles.
2. THE MCP_Server SHALL expose a `kb_get` tool that accepts an `article_id` and returns the full article content including frontmatter.
3. THE MCP_Server SHALL expose a `kb_create` tool that accepts a complete article, validates it against the Schema, and writes it to the KB_Repo as a new file.
4. IF a `kb_create` request contains an article that fails schema validation, THEN THE MCP_Server SHALL return a descriptive error identifying which required fields are missing or invalid, and SHALL NOT write the file.
5. THE MCP_Server SHALL expose a `kb_update` tool that accepts an `article_id`, a `changes` object, and a `changelog_entry` string, applies the changes to the article, appends the changelog entry to the article's Changelog section, and updates the `updated` field to the current date.
6. THE MCP_Server SHALL expose a `kb_audit` tool that accepts an optional `project` and optional `tags` filter and returns all articles where `audit_due` is earlier than the current date.
7. THE MCP_Server SHALL be installable and runnable via `uvx fieldnotes-mcp --kb-path <path>` without requiring a separate installation step.
8. THE MCP_Server SHALL read the KB_Repo from the local file system at the path provided via `--kb-path` and SHALL NOT require network access to a remote service.
9. WHEN the MCP_Server starts, IF the `--kb-path` directory does not contain a `fieldnotes.yml` file, THEN THE MCP_Server SHALL return an error describing the missing configuration and SHALL NOT start.

---

### Requirement 5: MCP Server — Fieldguide Operations

**User Story:** As an LLM agent executing a fieldguide, I want to load the guide, get relevant KB context, advance through steps, and submit feedback through MCP tools, so that I can execute any fieldguide without custom training or hardcoded guide logic.

#### Acceptance Criteria

1. THE MCP_Server SHALL expose a `fieldguide_load` tool that accepts a fieldguide `id` and returns the guide content, the current Session document state, and the next pending step.
2. WHEN `fieldguide_load` is called and no Session document exists for the fieldguide, THE MCP_Server SHALL create a new Session document with status `in_progress`, record the start time, and return it alongside the first step.
3. THE MCP_Server SHALL expose a `fieldguide_get_context` tool that accepts a fieldguide `id` and returns the full content of all KB articles listed in the fieldguide's `kb_references` field.
4. THE MCP_Server SHALL expose a `fieldguide_advance` tool that accepts a fieldguide `id`, a `step_id`, and a `result` object, marks the step as complete in the Session document, records the result, and returns the next pending step.
5. WHEN `fieldguide_advance` is called and the completed step is the last step in the fieldguide, THE MCP_Server SHALL update the Session document status to `complete` and record the completion time.
6. THE MCP_Server SHALL expose a `fieldguide_submit_feedback` tool that accepts a fieldguide `id`, a `step_id`, a `type` (one of: `correction`, `timing`, `alternative_approach`, `issue`), and a `content` string, and appends the feedback to a feedback file co-located with the fieldguide.
7. WHEN `fieldguide_advance` is called with a `step_id` that does not exist in the fieldguide, THE MCP_Server SHALL return an error and SHALL NOT modify the Session document.
8. THE MCP_Server SHALL expose a `fieldguide_review_feedback` tool that accepts a fieldguide `id`, reads the accumulated feedback file, groups feedback by step and type, and returns a structured list of improvement proposals — each containing the `step_id`, a summary of the feedback, a proposed change description, the source feedback IDs, and a confidence level (`high`, `medium`, `low`).
9. WHEN `fieldguide_review_feedback` produces improvement proposals, THE MCP_Server SHALL write them as entries to a backlog file co-located with the fieldguide. THE backlog file SHALL be append-only and human-triaged in Alpha.

---

### Requirement 6: Fieldguide Schema and Execution Protocol

**User Story:** As a guide author, I want to write fieldguides where each step declares its execution type and completion condition, so that any LLM with MCP access can execute the guide correctly without needing to infer what to do from prose.

#### Acceptance Criteria

1. THE Schema SHALL require every fieldguide step to declare a `type` field set to one of: `human_required`, `agent_executable`, `approval_gate`, or `verification`.
2. THE Schema SHALL require every fieldguide step to declare a `completion` block containing either a `command` and `expected` output, or a `condition` description for steps that cannot be verified programmatically.
3. THE Schema SHALL require every fieldguide step to declare an `advance_when` field set to one of: `verification_passes`, `human_confirms`, or `agent_completes`.
4. THE Schema SHALL allow every fieldguide step to declare an optional `feedback` block listing the feedback types the step accepts.
5. WHEN a step's `type` is `human_required`, THE executing agent SHALL present the step instructions to the human and SHALL NOT advance until the human confirms completion.
6. WHEN a step's `type` is `agent_executable`, THE executing agent SHALL execute the step, run the completion verification, and advance automatically if verification passes.
7. WHEN a step's `type` is `approval_gate`, THE executing agent SHALL present the planned action to the human, explain what will change, and SHALL NOT proceed until the human provides explicit confirmation.
8. WHEN a step's `type` is `verification`, THE executing agent SHALL run the completion command, compare output to `expected`, and report PASS or FAIL without advancing on FAIL.
9. THE Schema SHALL require fieldguides to declare a `kb_references` list of article IDs that provide context for executing the guide.
10. THE Schema SHALL require fieldguides to declare an `execution_model` block specifying whether the guide contains `human_steps`, `agent_steps`, and whether it `requires_approval`.
11. THE Schema SHALL require every fieldguide to include a Quick Summary block at the top of the guide body containing: `Outcome`, `Starting state`, `Ending state`, `Estimated time`, `Difficulty` (one of `beginner`, `intermediate`, `advanced`), and `Reversible` (one of `yes`, `no`, `partially`). This enables a reader to decide in seconds whether the guide matches their need.
12. THE Schema SHALL allow every fieldguide step to declare optional `tip`, `warning`, and `detailed_explanation` fields. WHEN present, the executing agent SHALL present `warning` prominently before executing the step, present `tip` inline with instructions, and SHALL NOT present `detailed_explanation` unless the human requests it or an error occurs.
13. THE Schema SHALL allow fieldguides to declare an optional `depends_on_fieldguides` list of other fieldguide IDs that must be complete before this guide can start.
14. WHEN `fieldguide_load` is called and the guide's `depends_on_fieldguides` list contains any fieldguide whose most recent session is not `complete`, THE MCP_Server SHALL return an error listing the unsatisfied dependencies and SHALL NOT create a new session for the requested guide.
15. THE executing agent SHALL follow the Agent Autonomy Rule: if a step is `agent_executable`, the agent performs the action itself; the agent SHALL NOT ask the human to perform actions the agent can perform. The agent SHALL involve the human only when the step type requires it (`human_required`, `approval_gate`), when a verification fails, or when an unexpected error occurs.
16. THE executing agent SHALL follow the Handoff Protocol when transitioning between step types, pausing for human input only at `human_required` and `approval_gate` boundaries, and halting progression on verification failures.

---

### Requirement 7: Session Documents

**User Story:** As an agent resuming a fieldguide on a new machine or in a new session, I want to load a session document that tells me exactly where execution left off, what decisions were made, and what issues are open, so that I never have to reconstruct context from conversation history.

#### Acceptance Criteria

1. THE Schema SHALL define a `session` article type with frontmatter fields: `id`, `type` (value: `session`), `fieldguide_id`, `status` (one of: `in_progress`, `complete`, `abandoned`), `started`, and `updated`.
2. THE Session SHALL record each completed step as an entry containing: `step_id`, `completed_at`, `result`, and any feedback submitted for that step.
3. THE Session SHALL record a `decisions` list of key-value pairs capturing values entered during execution (e.g., hostnames, IP addresses, wallet addresses) so subsequent steps and sessions can reference them without asking again.
4. THE Session SHALL record an `open_issues` list of issues encountered during execution that were not resolved before the session ended.
5. WHEN `fieldguide_load` is called, THE MCP_Server SHALL return the Session's `decisions` map alongside the next pending step so the executing agent has full context without reading conversation history.
6. THE Session document SHALL be stored as a file in the KB_Repo co-located with or adjacent to the fieldguide it tracks.
7. THE Session document SHALL be a valid KB article conforming to the Schema, so it can be read and updated via the standard `kb_get` and `kb_update` MCP tools.

---

### Requirement 8: Agent Hierarchy — Lead Researcher

**User Story:** As a KB owner, I want a lead researcher agent that serves as my single entry point to the KB, operates in explore or doer mode based on my intent, and proposes new SME agents when a domain grows deep enough to warrant a specialist, so that I have a coherent, controlled agent hierarchy rather than a collection of disconnected tools.

#### Acceptance Criteria

1. THE Lead_Researcher SHALL operate in Explore_Mode when the user's intent is to understand something new, map gaps, or think through a problem — and SHALL NOT rush to produce documents until the problem space is understood.
2. WHILE in Explore_Mode, THE Lead_Researcher SHALL ask clarifying questions, identify what is already known in the KB, identify what is not yet known, and propose a research plan before producing any new articles.
3. THE Lead_Researcher SHALL operate in Doer_Mode when the user's intent is direct execution — auditing articles, updating content, loading a fieldguide, or running a migration.
4. WHILE in Doer_Mode, THE Lead_Researcher SHALL delegate domain-specific work to the appropriate SME_Researcher and report results back to the user.
5. WHEN the Lead_Researcher determines that a knowledge domain has grown deep enough to warrant a dedicated specialist, THE Lead_Researcher SHALL propose creating a new SME_Researcher by calling the `agent_propose` MCP tool with a draft agent specification.
6. THE MCP_Server SHALL expose an `agent_propose` tool that accepts a draft SME agent specification and writes it to a pending proposals file for human review.
7. IF a new SME_Researcher proposal has not received explicit human approval, THEN THE Lead_Researcher SHALL NOT act as if the SME exists or delegate work to it.
8. THE Lead_Researcher specification SHALL be delivered as a Kiro steering file in `implementations/kiro/` that encodes explore mode and doer mode behavior as steering instructions.

---

### Requirement 9: Agent Hierarchy — SME Researchers

**User Story:** As a KB owner, I want SME researchers that own specific knowledge domains, sign every article they create or modify, and run audits on their own scope, so that every article has a clear responsible agent and domain expertise is encoded in the agent rather than reconstructed each session.

#### Acceptance Criteria

1. THE SME_Researcher SHALL sign every article it creates or modifies by setting the `agent` frontmatter field to its name and version in the format `agent-name/vX.Y`, and SHALL append an entry to the article's `modified_by` list recording its name/version, the current date, and the action performed.
2. WHEN an SME_Researcher creates a new knowledge article, THE SME_Researcher SHALL populate all required Schema fields, set `status` to `draft`, and set `audit_due` based on the article's volatility.
3. THE SME_Researcher SHALL only create or modify articles within its declared domain — it SHALL NOT modify articles owned by a different SME_Researcher without explicit instruction from the Lead_Researcher.
4. WHEN the Lead_Researcher requests an audit of a domain, THE SME_Researcher SHALL call `kb_audit` filtered to its domain, review each returned article, and produce an audit report as a `report` type article.
5. THE Framework_Repo SHALL provide an SME_Researcher template steering file that a new SME can be instantiated from by filling in domain, scope, and article ownership fields.
6. THE SME_Researcher template SHALL include instructions for: article creation, article update, audit execution, and escalation to the Lead_Researcher when a decision exceeds the SME's scope.

---

### Requirement 10: Cross-Project Knowledge Sharing

**User Story:** As an operator running multiple projects on shared physical hardware, I want knowledge articles about that hardware to be accessible to agents working on any of those projects, so that a decision made in one project context is visible to agents working in another project context without manual copy-paste.

#### Acceptance Criteria

1. THE Schema SHALL require every article to declare a `project` field in its frontmatter identifying which project the article primarily belongs to.
2. THE `kb_search` tool SHALL accept an optional `project` filter — WHEN omitted, THE MCP_Server SHALL search across all projects in the KB.
3. WHEN an article's facts are relevant to multiple projects (e.g., hardware shared between a staking node project and a network project), THE Schema SHALL allow an article to declare a `related_projects` list of additional project names.
4. THE Lead_Researcher SHALL, WHEN beginning work on a project, call `kb_search` without a project filter to surface articles from other projects that share relevant tags (e.g., same hardware, same domain).
5. THE Schema SHALL require hardware articles to include a `physical_id` tag identifying the physical device, so that agents working on different projects can find all articles about the same physical machine regardless of project.

---

### Requirement 11: Agent Portability — No Hardcoded Paths

**User Story:** As an agent definition author, I want agent steering files and knowledge base references to use relative paths or KB article IDs rather than absolute file system paths, so that the same agent definition works on any machine without modification.

#### Acceptance Criteria

1. THE Framework_Repo SHALL NOT contain any absolute file system paths in agent steering files, schema files, or MCP server configuration.
2. THE MCP_Server SHALL resolve all KB article references using article IDs looked up against the KB_Repo root, not file system paths.
3. WHEN an agent steering file references a knowledge article, THE reference SHALL use the article's `id` field, not a file path.
4. THE `fieldguide_get_context` tool SHALL resolve `kb_references` by article ID and return article content, so the executing agent never needs to know the file system layout of the KB_Repo.
5. THE MCP_Server SHALL accept the KB_Repo path as a runtime argument (`--kb-path`) and SHALL NOT require it to be hardcoded in any configuration file committed to the Framework_Repo.

---

### Requirement 12: Fieldguide Feedback Loop

**User Story:** As a fieldguide maintainer, I want feedback from real executions to be collected and stored in a structured way, so that I can review what went wrong, what took longer than expected, and what alternative approaches were found, and use that to improve the guide for the next person who runs it.

#### Acceptance Criteria

1. THE `fieldguide_submit_feedback` tool SHALL accept feedback of type `correction`, `timing`, `alternative_approach`, or `issue` and SHALL store it in a structured feedback file associated with the fieldguide.
2. THE feedback file SHALL record for each submission: `step_id`, `type`, `content`, `submitted_at`, and the session ID it came from.
3. THE feedback file SHALL be stored in the KB_Repo adjacent to the fieldguide it references and SHALL be a valid YAML or Markdown file readable without special tooling.
4. THE Schema SHALL allow fieldguide steps to declare which feedback types they accept via an optional `feedback.types` list — WHEN a step declares accepted types, THE MCP_Server SHALL validate that submitted feedback matches one of the declared types.
5. THE feedback file SHALL be append-only — THE MCP_Server SHALL never overwrite or delete existing feedback entries.
6. THE Lead_Researcher, WHILE in Doer_Mode, SHALL be able to request a feedback summary for a fieldguide by reading the feedback file and producing a structured report of findings grouped by step and type.
7. THE MCP_Server SHALL maintain a backlog file for each fieldguide that has received feedback. THE backlog file SHALL be stored in the KB_Repo adjacent to the fieldguide and its feedback file, and SHALL be a valid YAML or Markdown file readable without special tooling.
8. EACH backlog entry SHALL contain: a unique `item_id`, the `step_id` it relates to, a summary of the issue or improvement, a proposed change description, the source `feedback_ids`, a `priority` (one of: `high`, `medium`, `low`), a `status` (one of: `open`, `accepted`, `rejected`, `applied`), and a `created_at` timestamp.
9. THE backlog file SHALL be append-only — THE MCP_Server SHALL NOT overwrite or delete existing backlog entries. Status changes to existing entries (e.g., `open` → `accepted`) SHALL be recorded as updates to the entry, not as deletions and re-creations.
10. **Roadmap — post-Alpha:** THE feedback-to-improvement loop SHALL evolve toward agent-proposed fieldguide edits, where the Lead_Researcher in Doer_Mode can read the backlog, draft concrete changes to fieldguide steps, and present them to the human for approval before applying via `kb_update`. Alpha establishes the structured backlog and human triage workflow; post-Alpha automates the proposal-to-edit pipeline.

---

### Requirement 13: Minimal Example KB

**User Story:** As a new fieldnotes user, I want a working example KB that demonstrates the schema, a knowledge article, and a fieldguide with at least one step of each execution type, so that I can understand the system by reading a real example rather than only reading documentation.

#### Acceptance Criteria

1. THE Framework_Repo SHALL include a minimal example KB in `examples/minimal-kb/` containing at least one `knowledge` article and one `fieldguide` article.
2. THE example knowledge article SHALL demonstrate all required frontmatter fields, all required sections, a populated `sources` list, and a non-empty Changelog.
3. THE example fieldguide SHALL demonstrate at least one step of each execution type: `human_required`, `agent_executable`, `approval_gate`, and `verification`.
4. THE example fieldguide SHALL include a `kb_references` list pointing to the example knowledge article, demonstrating the cross-reference pattern.
5. THE example KB SHALL include a `fieldnotes.yml` configuration file with all required fields populated.
6. THE example KB SHALL be valid against the Schema — THE MCP_Server SHALL be able to start with `--kb-path examples/minimal-kb` and serve all example articles without errors.

---

### Requirement 14: Parser and Serializer Correctness

**User Story:** As a developer integrating with the fieldnotes MCP server, I want the YAML frontmatter parser and article serializer to be correct and round-trippable, so that articles are never silently corrupted when read and written by the MCP server.

#### Acceptance Criteria

1. WHEN the MCP_Server reads an article file, THE Parser SHALL parse the YAML frontmatter into a structured object without data loss.
2. WHEN the MCP_Server writes an article file, THE Serializer SHALL produce a valid YAML frontmatter block that, when parsed again, produces an equivalent structured object.
3. FOR ALL valid article objects, parsing then serializing then parsing SHALL produce an equivalent object (round-trip property).
4. WHEN the MCP_Server reads a file with malformed YAML frontmatter, THE Parser SHALL return a descriptive error identifying the file and the parse failure, and SHALL NOT return a partially-parsed object.
5. THE Serializer SHALL preserve the order of frontmatter fields as defined in the Schema, so that articles written by the MCP_Server are human-readable and diff-friendly.
6. THE Serializer SHALL preserve all existing article body content (sections below the frontmatter) when updating only frontmatter fields via `kb_update`.

---

### Requirement 15: Non-Functional Requirements

**User Story:** As a developer and operator of the fieldnotes MCP server, I want the server to be performant, safe, portable, and observable, so that I can trust it with my knowledge base and run it on any machine I work from.

#### Acceptance Criteria

##### Performance

1. THE MCP_Server SHALL handle a KB containing up to 500 articles without noticeable latency on `kb_search` and `kb_audit` operations.
2. THE MCP_Server SHALL build an in-memory index of all articles at startup and SHALL NOT re-scan the filesystem on every tool call.

##### Data Safety

3. THE MCP_Server SHALL NOT modify any file in the KB_Repo during read operations (`kb_search`, `kb_get`, `kb_audit`, `fieldguide_load`, `fieldguide_get_context`). Read operations SHALL be side-effect free.
4. THE MCP_Server SHALL write files atomically — a crash or interruption during a write operation SHALL NOT leave a partially-written or corrupted article in the KB_Repo.

##### Portability

5. THE MCP_Server SHALL run on macOS and Linux without platform-specific dependencies or conditional code paths.
6. THE MCP_Server SHALL require only Python 3.11+ and declared pip dependencies. No system-level packages beyond the Python interpreter SHALL be required.

##### Observability

7. THE MCP_Server SHALL log every write operation (`kb_create`, `kb_update`, `fieldguide_advance`, `fieldguide_submit_feedback`, `fieldguide_review_feedback`) with the tool name, article or fieldguide ID, and timestamp, to stderr or a configurable log output.

##### Schema Evolution

8. THE Schema SHALL maintain backward compatibility across all minor versions — an article valid under schema v1.0 SHALL remain valid under all v1.x releases.

---

### Requirement 16: Testing Strategy

**User Story:** As a developer working on the fieldnotes MCP server, I want a clear, practical testing strategy that verifies correctness without unnecessary ceremony, so that I can trust the server behaves correctly and catch regressions early.

#### Acceptance Criteria

##### Property-Based Testing

1. THE parser and serializer SHALL be tested using property-based testing (Hypothesis) to verify the round-trip property: for all valid article objects, parsing then serializing then parsing SHALL produce an equivalent object (as specified in Requirement 14, criterion 3).
2. THE property-based tests SHALL generate random valid articles covering all four article types (`knowledge`, `fieldguide`, `report`, `session`) and all volatility levels, to exercise edge cases that example-based tests would miss.

##### Behavior-Driven Test Structure

3. THE MCP tool tests SHALL follow a Given/When/Then structure in test naming and organization, using pytest. No BDD framework (e.g., behave) SHALL be required — the BDD discipline is in the test design, not the tooling.
4. EACH MCP tool SHALL have at least one test for its success path and at least one test for each documented error condition in the requirements.

##### Test Fixtures

5. THE test suite SHALL include a test KB fixture — a minimal KB directory on disk containing valid articles of each type — used by all integration tests. THE fixture SHALL be valid against the Schema and reusable across test runs.
6. THE test KB fixture SHALL be separate from the `examples/minimal-kb/` directory, so that example content and test content can evolve independently.

##### Integration Over Unit Testing

7. THE MCP tools SHALL be tested end-to-end through the tool interface, exercising the full path from tool call to file system operation and back. Internal functions SHALL NOT require separate unit tests unless they contain complex logic that is difficult to exercise through the tool interface alone.

##### Schema Validation Testing

8. THE schema validation logic SHALL be tested with both valid and invalid articles. Invalid article tests SHALL verify that the MCP_Server returns descriptive errors identifying which required fields are missing or invalid, as specified in Requirement 4, criterion 4.


---

### Requirement 17: Completion Verification

**User Story:** As a fieldguide author, I want verification steps to handle real-world command output, flaky timing, and ambiguous failures, so that the executing agent produces reliable PASS/FAIL/ERROR signals and attaches useful diagnostics without asking me to parse raw output.

#### Acceptance Criteria

##### Match Modes

1. THE completion block SHALL accept an `expected` field that is either a string (treated as exact match, preserving backward compatibility) or a structured object with `mode` and `value` fields.
2. THE Schema SHALL support the following match modes: `exact` (full string equality), `contains` (output contains the value as a substring), and `regex` (output matches the regex).
3. WHEN the `expected` field is a plain string, THE MCP_Server SHALL treat it as `mode: exact` for backward compatibility with fieldguides written before this requirement was added.

##### Retry Semantics

4. THE completion block SHALL accept an optional `retry` block containing `max_attempts` (integer, default 1) and `delay_seconds` (integer, default 0).
5. WHEN a `retry` block is declared, THE executing agent SHALL run the completion command up to `max_attempts` times with `delay_seconds` between attempts, reporting `fail` only if all attempts produce non-matching output.

##### Three-State Result Model

6. THE step result SHALL be one of three states: `pass` (verification matched), `fail` (command ran cleanly but output did not match), or `error` (the verification command itself failed with a non-zero exit code or could not be executed).
7. WHEN a verification returns `fail`, THE executing agent SHALL stop, report the failure output to the human, and SHALL NOT proceed to the next step. If an `on_failure.likely_causes` list is declared for the step, the agent SHALL present those causes alongside the failure.
8. WHEN a verification returns `error`, THE executing agent SHALL stop and report the command, exit code, and stderr. The agent SHALL NOT interpret an `error` as a `fail` and SHALL NOT re-run the verification automatically.

##### Diagnostic Execution

9. THE completion block SHALL accept an optional `on_failure` block containing `likely_causes` (list of strings) and `diagnostic_commands` (list of shell commands).
10. WHEN a step declares `on_failure.diagnostic_commands` and the verification returns `fail`, THE executing agent SHALL run each diagnostic command automatically and include their output in the failure report. Diagnostic commands are read-only by contract — THE Schema SHALL treat them as observation, not remediation.

##### Idempotence

11. THE step schema SHALL accept an optional `idempotent` boolean field (default `false`) declaring whether re-executing the step is safe.
12. WHEN a step has `idempotent: true` and the agent encounters a session-state conflict (step marked complete but verification now fails), THE executing agent MAY re-run the step after reporting the discrepancy and receiving explicit human approval.
13. WHEN a step has `idempotent: false` (or the field is absent) and any condition suggests re-running the step, THE executing agent SHALL NOT re-run it without explicit human approval.

---

### Requirement 18: Remediation Steps

**User Story:** As a fieldguide author, I want to declare how common verification failures should be remediated, so that the executing agent can handle expected failures according to my explicit authorization rather than either stopping immediately or improvising a fix.

#### Acceptance Criteria

##### Remediation Step Type

1. THE Schema SHALL define a new optional step type `remediation` that declares a fix for a preceding step's verification failure.
2. A `remediation` step SHALL declare a `remediates` field referencing the step ID it is the fix for. The `remediates` step must appear earlier in the fieldguide than the `remediation` step.
3. A `remediation` step SHALL declare its own `type` specifying the authority level: one of `agent_executable` (agent may execute the remediation autonomously), `approval_gate` (agent must present the remediation plan and wait for human approval), or `human_required` (only a human may perform the remediation).
4. A `remediation` step SHALL declare a `completion` block following the same rules as any other step, so the agent can verify the remediation succeeded.

##### Execution Rules

5. WHEN a step's verification returns `fail` AND a `remediation` step exists with `remediates` referencing the failed step, THE executing agent SHALL present the remediation to the human. The agent SHALL NOT execute the remediation automatically, even when its `type` is `agent_executable`, without first presenting its existence and intent to the human.
6. WHEN the human approves executing a remediation, THE executing agent SHALL execute it according to its declared `type` authority, verify completion, and THEN re-run the original step's verification to confirm the failure is resolved.
7. IF the original step's verification still fails after a remediation is completed, THE executing agent SHALL report the continued failure and SHALL NOT attempt additional remediations or improvisations.
8. THE Alpha implementation SHALL NOT support autonomous remediation without guide-author-declared `remediation` steps. Autonomous fixes to unexpected failures are explicitly out of scope for Alpha — see the Kiro Distinction in [TENETS.md](../TENETS.md).

##### Feedback Integration

9. WHEN a remediation is executed (successfully or not), THE MCP_Server SHALL record the remediation in the session document and SHALL attach structured feedback to the backlog file capturing: the failed step ID, the remediation step ID, the result of the remediation, and any human input or override. This feedback informs whether the guide itself should be updated — a frequently-used remediation may be a sign that the original step should be rewritten.