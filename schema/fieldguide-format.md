# Fieldguide Format Schema

> fieldnotes schema v1.0 — Alpha

This document defines the structure, execution protocol, and authoring rules for fieldguide articles. A fieldguide is a structured human-AI execution guide — not just documentation, but an executable protocol that any MCP-compatible LLM can load and run.

The core idea: the guide contains the instructions for how to execute it. The behavior lives in the document, not in the agent. Any LLM that can call MCP tools becomes a runtime.

**Audience:** Agents executing fieldguides, humans authoring fieldguides, and the MCP server's fieldguide tools.

**Related:** [article-format.md](article-format.md) for universal frontmatter fields. [audit-rules.md](audit-rules.md) for volatility and audit behavior.

---

## Frontmatter

Fieldguide articles use all universal frontmatter fields defined in [article-format.md](article-format.md), plus the following fieldguide-specific fields:

| Field | Type | Required | Description |
|---|---|---|---|
| `kb_references` | list of strings | Yes | Article IDs that provide context for executing this guide. Resolved by `fieldguide_get_context`. |
| `depends_on_fieldguides` | list of strings | No | Fieldguide IDs that must be completed before this guide can start. See [Fieldguide Dependencies](#fieldguide-dependencies). |
| `execution_model` | block | Yes | Declares execution characteristics. See below. |

### execution_model Block

| Field | Type | Required | Description |
|---|---|---|---|
| `human_steps` | boolean | Yes | Whether the guide contains `human_required` steps. |
| `agent_steps` | boolean | Yes | Whether the guide contains `agent_executable` steps. |
| `requires_approval` | boolean | Yes | Whether the guide contains `approval_gate` steps. |

---

## Required Sections

Fieldguide articles must include the following sections in order:

```
## Quick Summary
## Purpose
## Prerequisites
## Execution Summary
## Phases
## Changelog
```

### Quick Summary

A fixed-format block at the top of the guide body, designed for 10-second scanning. Both humans and agents use this to answer "is this the guide I need?" without reading further.

Required format:

```markdown
**Outcome:** [one sentence — what this guide accomplishes]
**Starting state:** [one sentence — what must be true to begin]
**Ending state:** [one sentence — what will be true when done]
**Estimated time:** [range, e.g., 45-60 minutes]
**Difficulty:** [beginner | intermediate | advanced]
**Reversible:** [yes | no | partially]
```

Guidelines:
- Keep each line to one sentence. If you can't, the scope is too broad — consider splitting the guide.
- `Starting state` and `Ending state` describe the system, not the actions. "SSH is configured with key-only auth" (state), not "SSH has been hardened" (action).
- `Reversible: partially` requires a follow-up note in Purpose explaining which parts are reversible and which aren't.

### Purpose

What the guide accomplishes, in narrative form. One or two paragraphs. Written for both humans and agents — an agent reading this should understand the goal well enough to make judgment calls during execution.

### Prerequisites

What must be true before starting. Hardware available, software installed, access credentials obtained, prior fieldguides completed. Each prerequisite should be verifiable — either a command to run or a condition to check.

### Execution Summary

Overview of the phases, estimated total time, and which step types are involved. Complements the Quick Summary with phase-level detail.

### Phases

The main body. Contains one or more phases, each containing one or more steps. See [Phase Structure](#phase-structure) and [Step Schema](#step-schema).

### Changelog

Append-only record of changes to the guide. Same format as all article changelogs.

---

## Phase Structure

Phases group related steps into logical units. Each phase has:

```yaml
## Phase 1: Hardware Preparation

Description of what this phase accomplishes and why.

### Steps

- id: phase-1-step-1
  title: "Verify hardware specifications"
  ...
```

Phases are numbered sequentially. Steps within a phase are executed in order unless `depends_on` specifies otherwise.

---

## Step Schema

Each step in a fieldguide declares its execution type, instructions, completion condition, and advancement rule.

```yaml
- id: phase-3-step-2
  title: "Enable UFW firewall"
  type: approval_gate
  instructions: |
    Enable UFW to enforce the configured firewall rules.
    Command: sudo ufw enable
  warning: |
    If SSH rules are misconfigured, enabling UFW will immediately lock you
    out of the server. Verify the SSH rule is present before approving.
  tip: |
    You can test SSH access from a second terminal before approving,
    so you know connectivity works before the firewall takes effect.
  completion:
    command: "ssh admin@${hostname} 'sudo ufw status | grep -q \"Status: active\" && echo PASS || echo FAIL'"
    expected: "PASS"
  advance_when: human_confirms
  feedback:
    types: [correction, timing, issue]
  depends_on: []
```

### Step Fields

| Field | Type | Required | Description |
|---|---|---|---|
| `id` | string | Yes | Unique within the guide. Format: `phase-N-step-N`. |
| `title` | string | Yes | Human-readable step title. |
| `type` | enum | Yes | One of: `human_required`, `agent_executable`, `approval_gate`, `verification`. |
| `instructions` | string | Yes | Markdown instructions for executing the step. Written for the actor (human or agent) identified by `type`. |
| `completion` | block | Yes | How to verify the step is done. See [Completion Block](#completion-block). |
| `advance_when` | enum | Yes | One of: `verification_passes`, `human_confirms`, `agent_completes`. |
| `tip` | string | No | Helpful context or shortcut. Presented inline with instructions before executing the step. |
| `warning` | string | No | Critical information (risks, common mistakes, things not to skip). Presented prominently before executing the step. |
| `detailed_explanation` | string | No | The "why" behind the step. Available on request — NOT shown by default during execution. |
| `feedback` | block | No | Feedback types this step accepts. See [Feedback Block](#feedback-block). |
| `depends_on` | list of strings | No | Step IDs that must complete before this step can start. Default: previous step in sequence. |

### Step Types

| Type | Actor | Description |
|---|---|---|
| `human_required` | Human | The agent cannot perform this action. Physical actions, credential entry, or decisions requiring human judgment the guide author decided not to automate. |
| `agent_executable` | Agent | The agent performs this autonomously. Executes, verifies, advances. No human involvement unless verification fails. |
| `approval_gate` | Agent (with human consent) | The agent can perform this but must not without explicit human permission. Destructive, irreversible, or cost-incurring operations. |
| `verification` | Agent (observe only) | The agent checks state and reports. Does not attempt to fix failures. |

### Completion Block

Every step must declare how completion is verified. Two forms:

**Command-based verification** (preferred when possible):

```yaml
completion:
  command: "sudo ufw status | grep -q 'Status: active' && echo PASS || echo FAIL"
  expected: "PASS"
```

**Condition-based verification** (when programmatic verification isn't possible):

```yaml
completion:
  condition: "Human confirms the Ethernet cable is connected to port 2 on the switch"
```

### Feedback Block

Optional. Declares which feedback types the step accepts. If declared, the MCP server validates submitted feedback against this list.

```yaml
feedback:
  types: [correction, timing, alternative_approach, issue]
```

Valid feedback types: `correction`, `timing`, `alternative_approach`, `issue`.

### advance_when Values

| Value | Meaning |
|---|---|
| `verification_passes` | The completion command runs and output matches `expected`. |
| `human_confirms` | The human explicitly confirms the step is done. |
| `agent_completes` | The agent marks the step done after executing it (no external verification). |

### Tip, Warning, and Detailed Explanation

These optional fields make steps easier to execute and understand without cluttering the core instructions.

**`tip`** — helpful context or a shortcut. Use when there's a non-obvious but useful piece of information.

**`warning`** — critical information. Use when there's a risk of data loss, a common mistake that causes failure, or something that must not be skipped.

**`detailed_explanation`** — the "why" behind the step. Use when the reasoning is non-trivial but not needed to execute the step correctly. Available on request; not shown by default.

**Guidelines for when to use each:**

| Field | Use when | Don't use when |
|---|---|---|
| `tip` | There's a shortcut or helpful context | The information is essential to completing the step (put it in instructions) |
| `warning` | Skipping or misdoing this step causes real harm | The risk is minor or obvious |
| `detailed_explanation` | Reasoning is non-trivial and useful for debugging or learning | The explanation fits in one sentence (put it in instructions) |

Keep these fields focused. A step with all three is usually a sign the instructions themselves need rewriting.

---

## Execution Protocol

This section defines how an executing agent processes a fieldguide. Any MCP-compatible LLM follows this protocol.

### The Execution Loop

```
1. fieldguide_load(id)          → guide content + session state + next pending step
2. fieldguide_get_context(id)   → referenced KB articles loaded as context
3. Read next pending step
4. Execute based on type (see rules below)
5. fieldguide_advance(id, step_id, result)
6. If feedback → fieldguide_submit_feedback(...)
7. Repeat from step 3 until all steps complete
```

### Execution Rules by Step Type

#### agent_executable

The agent executes the step fully and autonomously.

1. Read the instructions.
2. Execute the actions described.
3. Run the completion verification.
4. If verification passes → call `fieldguide_advance` with the result. Proceed to next step.
5. If verification fails → report the failure to the human. Do NOT attempt to fix it autonomously. Do NOT proceed to the next step. Wait for human guidance.

**The agent MUST NOT ask the human to perform actions that the agent can perform itself.** If the step is `agent_executable`, the agent does the work. It does not present commands for the human to run. It does not ask for confirmation of intermediate results. The completion verification handles correctness.

#### human_required

The agent presents the step to the human and waits.

1. Present the step title and instructions clearly.
2. Explain *why* this requires human action (physical action, credentials, judgment call).
3. Wait for the human to confirm completion.
4. Do NOT attempt to perform the action. Do NOT offer to "try it anyway."
5. Once the human confirms → call `fieldguide_advance`. Proceed to next step.

#### approval_gate

The agent presents a plan and waits for explicit consent.

1. Present the planned action in detail.
2. Explain what will change, what the impact is, and whether it's reversible.
3. Wait for explicit human confirmation ("yes", "approved", "proceed").
4. Do NOT proceed on ambiguous responses. If unclear, ask again.
5. Do NOT present the approval gate as optional or suggest skipping it.
6. Once approved → execute the action, run verification, call `fieldguide_advance`.
7. If the human declines → record the decision in the session. Do NOT proceed with the step. Ask the human how to continue.

#### verification

The agent checks state and reports. It does not act.

1. Run the completion command.
2. Compare output to `expected`.
3. Report PASS or FAIL.
4. If PASS → call `fieldguide_advance`. Proceed to next step.
5. If FAIL → report the failure. Do NOT attempt to fix the issue. Do NOT proceed. Wait for human guidance.

### Agent Autonomy Rule

**The executing agent MUST maximize autonomous execution.** Specifically:

- If a step is `agent_executable`, the agent does it. Period. No asking the human to "run this command" or "check this output."
- The agent only involves the human when: (a) the step type explicitly requires it (`human_required`, `approval_gate`), (b) a verification fails, or (c) an unexpected error occurs that the instructions don't cover.
- Between steps, the agent does not pause for confirmation unless the next step is `human_required` or `approval_gate`. Sequential `agent_executable` steps flow without interruption.

### Handling Tip, Warning, and Detailed Explanation

When a step has these optional fields, the executing agent follows these rules:

- **`warning`:** Present to the human before executing the step. Make it visually distinct (e.g., prefix with "⚠️ WARNING:"). Do this even for `agent_executable` steps — the human should see the warning before the action happens.
- **`tip`:** Present to the human inline with the step instructions. Lower visual weight than warnings.
- **`detailed_explanation`:** Do NOT present by default. Keep the execution flow tight. Present when:
  - The human explicitly asks "why this step?" or "explain this step"
  - An error occurs and the agent needs context to diagnose or report it clearly
  - A verification fails and the detailed explanation might help the human decide what to do next

### Handoff Protocol

When transitioning between step types, the agent follows these rules:

| From | To | Agent behavior |
|---|---|---|
| `agent_executable` | `agent_executable` | Proceed immediately. No pause, no confirmation. |
| `agent_executable` | `human_required` | Present the next step's instructions. Explain what the human needs to do and why the agent can't. Wait. |
| `agent_executable` | `approval_gate` | Stop. Present the plan. Wait for explicit approval. |
| `agent_executable` | `verification` | Run the check immediately. Report result. |
| `human_required` | `agent_executable` | Wait for human confirmation that the manual step is done. Then proceed autonomously. |
| `human_required` | `human_required` | Present the next step. Wait for confirmation. |
| `human_required` | `approval_gate` | Present the plan. Wait for approval. |
| `human_required` | `verification` | Wait for human confirmation of the previous step. Then run the check. |
| `approval_gate` | any | After approval and execution, follow the rules for the next step type. |
| `verification` (PASS) | any | Follow the rules for the next step type. |
| `verification` (FAIL) | any | Stop. Report failure. Wait for human guidance. Do NOT proceed. |

### Error Handling

When something unexpected happens during execution:

1. **Command fails with unexpected error:** Report the error, the command that failed, and the output. Do NOT retry automatically. Wait for human guidance.
2. **Step instructions are ambiguous:** Ask the human for clarification before acting. Do NOT guess.
3. **Dependency not met:** If a step's `depends_on` references an incomplete step, skip it and report the dependency. Do NOT execute out of order.
4. **Session state conflict:** If the session shows a step as already completed but the verification now fails, report the discrepancy. Do NOT silently re-execute.

---

## Session Interaction

Each completed step is recorded in the session document as:

```yaml
completed_steps:
  - step_id: phase-1-step-1
    completed_at: 2026-05-01T14:30:00Z
    result:
      status: pass
      output: "PASS"
    feedback: []
  - step_id: phase-1-step-2
    completed_at: 2026-05-01T14:35:00Z
    result:
      status: pass
      output: "Rules updated"
    feedback:
      - type: timing
        content: "Took 10 minutes, not 5 as estimated"
```

Decisions made during execution (hostnames, IPs, configuration values) are recorded in the session's `decisions` map so subsequent steps and future sessions can reference them without re-asking.

```yaml
decisions:
  hostname: "rp-node-01"
  eth_client: "nethermind"
  consensus_client: "nimbus"
  ssh_port: "22"
```

---

## Feedback File Format

Feedback is stored in a YAML file co-located with the fieldguide. Filename: `{fieldguide-id}.feedback.yml`.

```yaml
feedback:
  - step_id: phase-1-step-2
    type: timing
    content: "Step took 10 minutes, guide estimates 5"
    submitted_at: 2026-05-01T14:36:00Z
    session_id: session-rocketpool-setup-001
  - step_id: phase-3-step-1
    type: correction
    content: "Command should use --network=mainnet not --network=prater"
    submitted_at: 2026-05-01T15:20:00Z
    session_id: session-rocketpool-setup-001
```

The feedback file is append-only. The MCP server never overwrites or deletes existing entries.

---

## Backlog File Format

Improvement items derived from feedback are stored in a YAML file co-located with the fieldguide. Filename: `{fieldguide-id}.backlog.yml`.

```yaml
backlog:
  - item_id: backlog-001
    step_id: phase-1-step-2
    summary: "Time estimate is inaccurate"
    proposed_change: "Update time estimate from 5 min to 10-15 min"
    feedback_ids: [feedback-001, feedback-005]
    priority: low
    status: open
    created_at: 2026-06-15T10:00:00Z
  - item_id: backlog-002
    step_id: phase-3-step-1
    summary: "Wrong network flag in command"
    proposed_change: "Change --network=prater to --network=mainnet"
    feedback_ids: [feedback-002]
    priority: high
    status: accepted
    created_at: 2026-06-15T10:00:00Z
```

The backlog file is append-only. Status changes to existing entries (`open` → `accepted` → `applied`) are recorded as updates to the entry, not as deletions and re-creations.

### Backlog Entry Fields

| Field | Type | Description |
|---|---|---|
| `item_id` | string | Unique identifier. Format: `backlog-NNN`. |
| `step_id` | string | The step this improvement relates to. |
| `summary` | string | Brief description of the issue or improvement. |
| `proposed_change` | string | What should change in the fieldguide. |
| `feedback_ids` | list of strings | Source feedback entries that prompted this item. |
| `priority` | enum | One of: `high`, `medium`, `low`. |
| `status` | enum | One of: `open`, `accepted`, `rejected`, `applied`. |
| `created_at` | datetime | When the backlog item was created. |

---

## Guide Authoring Rules

These rules are for humans and agents writing fieldguides. Following them ensures guides are executable, efficient, and don't waste the human's time on things the agent can handle.

### Step Type Selection

**Default to `agent_executable`** unless there is a specific reason the agent cannot or should not perform the action.

| Use this type | When |
|---|---|
| `agent_executable` | The agent can perform the action and verify the result. This is the default. |
| `human_required` | The action requires physical access (plug in a cable, press a button), credentials the agent doesn't have, or human judgment the guide author explicitly decided not to automate. |
| `approval_gate` | The action is destructive, irreversible, or cost-incurring. The agent can do it but must not without consent. |
| `verification` | The step is purely a state check — no action, just observation. Used after critical steps to confirm state before proceeding. |

**NEVER use `human_required` as a lazy default.** If the agent can run a command, the agent runs the command. If the agent can edit a file, the agent edits the file. `human_required` is for things the agent genuinely cannot do — not things the author didn't bother to automate.

### Writing Good Instructions

- Write instructions for the actor. If the step is `agent_executable`, write for the agent — include the exact commands, file paths, and expected outcomes. If the step is `human_required`, write for the human — be clear about what to do and how to confirm it's done.
- Include the *why* when it affects execution. For background context that isn't execution-critical, use `detailed_explanation` instead of cluttering the instructions.
- For `agent_executable` steps, prefer explicit commands over prose descriptions. "Run `sudo ufw allow 30303/tcp`" is better than "Allow the execution client port through the firewall."
- For `approval_gate` steps, explain the consequences in the instructions. Use `warning` for the risk of things going wrong.

### Writing Good Completion Conditions

- Prefer command-based verification over condition-based. Commands are unambiguous and machine-verifiable.
- The completion command should check the *result*, not the *action*. Check that the firewall is active, not that the `ufw enable` command was run.
- For condition-based completion, be specific. "Human confirms the cable is connected" is better than "Cable is connected."
- The `expected` value should be a simple, exact string. Avoid complex output parsing in the completion command — pipe through grep or awk to produce a clean PASS/FAIL.

### Structuring Phases

- Group steps by logical unit of work, not by actor. A phase about "Network Configuration" might have agent steps, a verification, and an approval gate — that's fine.
- Keep phases to 3–7 steps. Fewer than 3 and the phase is probably unnecessary grouping. More than 7 and it should be split.
- Place `verification` steps after critical actions, especially before phase boundaries. Catching a failure early prevents wasted work in later phases.
- Place `approval_gate` steps before destructive operations, not after. The human should approve the plan, not ratify a fait accompli.

### kb_references

List every knowledge article the executing agent needs as context. Be specific — don't reference the entire KB. The agent loads these via `fieldguide_get_context`, so every reference adds to the context window.

- Reference hardware articles if the guide involves specific devices
- Reference protocol/software articles if the guide configures specific software
- Reference OS articles if the guide assumes a specific OS configuration
- Do NOT reference other fieldguides here — use `depends_on_fieldguides` for that

### Reference Knowledge, Don't Duplicate It

The fieldguide is the *protocol*. Knowledge articles are the *context*. Keep them separate.

- If a step needs explanation of what a component is, or why a decision was made, that explanation belongs in a **knowledge article**. The fieldguide references it via `kb_references`.
- Fieldguides contain: what to do, in what order, how to verify completion, and warnings/tips specific to executing the steps.
- Fieldguides do NOT contain: general explanations of components, rationale for architectural decisions, or source URLs for facts. Those live in knowledge articles.

If you find yourself writing several paragraphs of background in a fieldguide, extract it into a knowledge article and reference it. This keeps guides readable and prevents context rot — when the knowledge changes, the guide doesn't silently go stale with duplicated outdated explanations.

**Use `detailed_explanation` on a step for execution-specific context** (e.g., "how to debug this if it fails"). Use knowledge articles for general explanations that apply beyond this guide.

### Splitting Large Workflows Across Multiple Fieldguides

When setting up a complex system — a full homelab, a multi-node cluster, a staged migration — resist the urge to write one mega-fieldguide. Split the work into multiple guides connected by `depends_on_fieldguides`.

**Why split:**
- Shorter guides are easier to execute in one session
- Components become reusable (set up a second node without redoing network setup)
- Maintenance is localized (the node software guide changes quarterly; the network setup rarely does)
- Session documents stay manageable
- Failures during one phase don't invalidate progress in another

**How to decide boundaries:**
- **By lifecycle:** things that change together stay together. Things that change at different rates get separated.
- **By reusability:** if a phase could apply to a second instance (e.g., another server), it's a candidate for its own guide.
- **By actor mix:** if a phase is entirely `human_required` and the next phase is entirely `agent_executable`, that's a natural boundary.

**Example — splitting a homelab setup:**
- `fieldguide-homelab-network-setup` — VLAN config, firewall, switch setup (rarely changes)
- `fieldguide-server-hardware-install` — physical install, BIOS, cabling (per-device, reusable)
- `fieldguide-server-os-setup` — Ubuntu install, SSH hardening (per-device, reusable)
- `fieldguide-rocketpool-node-setup` — node software (changes with each Rocket Pool release)

Each declares its dependencies. The agent verifies dependencies are complete before starting any guide.

**Meta-fieldguide pattern (optional):** A top-level orchestration fieldguide whose steps reference other fieldguides can provide the end-to-end workflow without a monolithic document. Not required — declaring dependencies is sufficient — but useful when you want a single entry point for a complex workflow.

### Fieldguide Dependencies

The `depends_on_fieldguides` field declares that this guide cannot be executed until the referenced guides are complete.

```yaml
depends_on_fieldguides:
  - fieldguide-homelab-network-setup
  - fieldguide-server-hardware-install
  - fieldguide-server-os-setup
```

**Enforcement:**

When `fieldguide_load` is called, the MCP server checks each entry in `depends_on_fieldguides`:

1. For each dependency ID, look up the most recent session document for that fieldguide.
2. If the session exists and its `status` is `complete`, the dependency is satisfied.
3. If the session does not exist, or its status is `in_progress` or `abandoned`, the dependency is NOT satisfied.
4. If any dependencies are unsatisfied, `fieldguide_load` returns an error listing the unsatisfied dependencies and SHALL NOT create a new session for the requested guide.

**Authoring rule:** Dependencies should describe *state* requirements, not *version* requirements. "The server must have Ubuntu installed" is a state requirement satisfied by `fieldguide-server-os-setup`. "The server must have Ubuntu 24.04 specifically" is a version requirement — that belongs in Prerequisites, not in `depends_on_fieldguides`.

---

## Complete Example

```yaml
---
id: fieldguide-example-server-setup
type: fieldguide
title: "Example Server Setup — Ubuntu 24.04"
project: example-project
tags:
  domain: [guide, os, network]
  volatility: slow
  status: draft
agent: lead-researcher/v1.0
modified_by:
  - agent: lead-researcher/v1.0
    date: 2026-04-20
    action: created
created: 2026-04-20
updated: 2026-04-20
kb_references:
  - hardware-example-server
  - os-ubuntu-2404
execution_model:
  human_steps: true
  agent_steps: true
  requires_approval: true
---

## Quick Summary

**Outcome:** A fresh Ubuntu 24.04 server with key-only SSH and an active UFW firewall.
**Starting state:** Unboxed hardware with network cable connected and a prepared Ubuntu install USB.
**Ending state:** Server is reachable via SSH with key-only auth, password auth disabled, UFW enabled allowing only port 22.
**Estimated time:** 45-60 minutes
**Difficulty:** intermediate
**Reversible:** partially

## Purpose

Set up a fresh Ubuntu 24.04 server with SSH access, firewall, and basic hardening. The OS installation is not reversible without wiping the disk. SSH and firewall configuration are reversible.

## Prerequisites

- Physical access to the server for initial OS installation
- Ubuntu 24.04 Server ISO prepared on USB drive
- Network cable connected to the target switch port
- SSH key pair generated on the management machine

## Execution Summary

Three phases, estimated 45–60 minutes total.
- Phase 1: OS Installation (human_required — physical access needed)
- Phase 2: Network and SSH Configuration (agent_executable)
- Phase 3: Firewall and Hardening (mixed — agent steps with approval gate)

## Phases

## Phase 1: OS Installation

Physical installation of Ubuntu 24.04 on the target hardware.

### Steps

- id: phase-1-step-1
  title: "Install Ubuntu 24.04 from USB"
  type: human_required
  instructions: |
    Boot the server from the prepared USB drive. Follow the Ubuntu Server
    installer. Select the following options:
    - Minimal installation
    - Use entire disk (no LVM)
    - Set hostname to the value agreed in prerequisites
    - Create the admin user account
    - Enable OpenSSH server during installation
  tip: |
    If the server doesn't boot from USB, check the BIOS boot order.
    On Beelink devices, press F7 at boot to open the boot menu directly.
  completion:
    condition: "Human confirms Ubuntu 24.04 is installed and the server has rebooted to the login prompt"
  advance_when: human_confirms
  feedback:
    types: [timing, issue]

- id: phase-1-step-2
  title: "Verify OS installation"
  type: verification
  instructions: |
    SSH into the server and verify the OS version.
  completion:
    command: "ssh admin@${hostname} 'lsb_release -rs'"
    expected: "24.04"
  advance_when: verification_passes

## Phase 2: Network and SSH Configuration

Configure SSH hardening and verify network connectivity.

### Steps

- id: phase-2-step-1
  title: "Harden SSH configuration"
  type: agent_executable
  instructions: |
    Edit /etc/ssh/sshd_config to disable password authentication
    and root login. Set:
    - PasswordAuthentication no
    - PermitRootLogin no
    - PubkeyAuthentication yes
    Restart the SSH service after changes.
  warning: |
    Before disabling password authentication, verify your SSH key is
    authorized on the server. If the key is missing, disabling password
    auth will lock you out.
  detailed_explanation: |
    OpenSSH reads /etc/ssh/sshd_config at service start. Changes take
    effect after `systemctl restart ssh`. Active connections are not
    dropped on restart, so you can safely test a new terminal after
    the restart to verify key auth works before closing the original.
  completion:
    command: "ssh admin@${hostname} 'sudo sshd -T | grep -c \"passwordauthentication no\"'"
    expected: "1"
  advance_when: verification_passes
  feedback:
    types: [correction, alternative_approach]

## Phase 3: Firewall and Hardening

Configure UFW firewall. Includes an approval gate before enabling.

### Steps

- id: phase-3-step-1
  title: "Configure UFW rules"
  type: agent_executable
  instructions: |
    Configure UFW to allow SSH (port 22) and deny all other incoming traffic.
    Do NOT enable UFW yet — that requires approval.
    Commands:
    - sudo ufw default deny incoming
    - sudo ufw default allow outgoing
    - sudo ufw allow 22/tcp
  completion:
    command: "ssh admin@${hostname} 'sudo ufw status verbose | grep -c \"22/tcp\"'"
    expected: "1"
  advance_when: verification_passes

- id: phase-3-step-2
  title: "Approve firewall activation"
  type: approval_gate
  instructions: |
    UFW is configured with the following rules:
    - Default: deny incoming, allow outgoing
    - Allow: SSH (port 22/tcp)

    Enabling UFW will immediately enforce these rules.
    Approve to enable UFW, or decline to review the rules first.
  warning: |
    If SSH rules are misconfigured, enabling UFW will immediately lock you
    out of the server. Verify the SSH rule is present before approving.
  tip: |
    Open a second SSH session before approving. If the firewall locks
    out new connections, your existing session will remain open and
    you can roll back.
  completion:
    command: "ssh admin@${hostname} 'sudo ufw status | grep -q \"Status: active\" && echo PASS || echo FAIL'"
    expected: "PASS"
  advance_when: human_confirms

- id: phase-3-step-3
  title: "Verify firewall is active"
  type: verification
  instructions: |
    Confirm UFW is active and SSH connectivity is maintained.
  completion:
    command: "ssh admin@${hostname} 'sudo ufw status | head -1'"
    expected: "Status: active"
  advance_when: verification_passes
  feedback:
    types: [issue]

## Changelog

- 2026-04-20: Initial draft. Created by lead-researcher/v1.0.
```
