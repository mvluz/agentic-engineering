# Tooling Strategy

This architectural policy determines which responsibilities use Codex native capabilities, repository instructions, Skills, MCP, deterministic custom tooling, or external frameworks. It does not install, configure, or implement tooling.

## Core Principle

Use:

Native runtime and authoritative documentation -> explicit procedure routing -> MCP for authenticated external capabilities -> thin deterministic tooling when justified -> external framework only when it clearly adds value

Do not create custom infrastructure merely because it is possible. Prefer reliable existing capabilities when they satisfy Agentic Engineering governance.

## Codex Native Runtime

Codex native capabilities are the preferred initial runtime for Root Engineering Copilot executions, Lead Engineer executions, Specialist Agent Runs, isolated subagent execution, repository interaction, filesystem and shell capabilities, supported model and reasoning configuration, Skills, MCP integrations, and approved tool access.

Agentic Engineering defines how these capabilities must be used. Codex is the runtime; Agentic Engineering remains the engineering governance and orchestration model.

## No Custom Agent Runtime in V1

Do not initially build a custom orchestration runtime in Python, JavaScript, or another framework to reproduce capabilities already available in Codex. Avoid custom agent schedulers, subagent runtimes, conversation buses, workflow engines, model routers, and context inheritance engines.

A custom orchestration runtime is considered only after a demonstrated Codex limitation materially prevents an approved requirement.

## Repository Instructions

Repository-local instructions and durable documentation provide repository-specific memory, including `AGENTS.md`, architecture and testing documentation, coding standards, API contracts, ADRs, and project conventions.

Repository instructions answer: “How should an agent work in this repository?” They must not become operational task-progress trackers; operational progress belongs in the external work-management system.

## Skills

Skills are optional, evidence-driven procedure or capability packages. Agentic Engineering v1 operates through authoritative documentation and explicit procedure routing without custom Skills by default. A Skill may be introduced only when it demonstrates benefit over that approach, as governed by [governance/skills-strategy.md](skills-strategy.md).

## MCP

MCP provides authenticated interaction with external systems such as GitHub, AWS, Figma, and other engineering services. It exposes a capability without exposing the underlying credential.

Secure capability, never credentials. Agents must never receive, store, log, document, or commit secrets merely to use an external capability.

### GitHub MCP Direction

GitHub is the operational work-management source of truth for Issues, GitHub Projects, repository references, operational task state, blockers, task completion, and next eligible work. GitHub MCP is the validated authenticated capability used primarily by the Root Engineering Copilot. The approved workflow is defined in [governance/github-workflow.md](github-workflow.md); command and automation implementation remain deferred.

## Deterministic Custom Tooling

Use small scripts or programs when a responsibility is deterministic and does not benefit from LLM reasoning. Potential examples include collecting runtime usage, calculating token totals and API-Equivalent Reference Cost, applying pricing snapshots, USD/BRL conversion, usage reports, schema validation, deterministic state checks, and future budget enforcement.

If a task can be reliably determined mechanically, prefer deterministic code over LLM reasoning. Custom tooling remains thin and focused.

### Telemetry

Execution telemetry is a strong candidate for deterministic tooling:

Codex runtime -> usage data -> deterministic collector -> execution ledger -> reports

Agents may read and summarize measured telemetry but must not invent or estimate token usage when measurable runtime data is unavailable. Runtime measurement has been validated; thin telemetry implementation remains deferred.

## Spec Kit

Spec Kit is not a dependency of Agentic Engineering v1. The system owns its Spec-Driven Development lifecycle and governance independently. The public Spec Kit project may be studied as a reference implementation, and useful patterns may be adapted independently where they fit existing governance.

Useful patterns include separation of specification, planning, tasks, and implementation; requirement clarification before expensive planning; governance validation during planning; consistency analysis across upstream artifacts; task derivation from specification and planning; targeted context retrieval; materializing planned tasks as GitHub work; and convergence validation after implementation.

Do not copy behavior that conflicts with Agentic Engineering governance.

### Behaviors Not Adopted

The initial system does not adopt as defaults:

- parallel task execution;
- `[P]` parallel task semantics;
- repository Markdown checkboxes as operational work state;
- implementing an entire task list in one run;
- automatic continuation between Atomic Tasks;
- bypassing the Copilot / Lead / Specialist authority hierarchy.

Agentic Engineering remains sequential in v1.

## Independent Process Ownership

Agentic Engineering must remain usable without Spec Kit. Its core lifecycle remains:

Product Requirement -> Technical Specification -> Implementation Plan -> Atomic Tasks -> Consistency Gate -> operational GitHub work -> one Atomic Task execution -> validation -> STOP -> eventual Convergence Check -> final engineering review

External tooling may help materialize this lifecycle but must not own or redefine it.

## Tool Selection Guide

| Need | Preferred Mechanism |
|------|---------------------|
| Reasoning, planning, implementation, evaluation | Codex Agent / Subagent |
| Repository-specific instructions and memory | `AGENTS.md` + repository docs |
| Reusable procedure or capability knowledge | Authoritative documentation and explicit routing; optional Skill only when justified |
| Authenticated external system interaction | MCP |
| Deterministic calculation, collection, or validation | Thin custom script/tool |
| External SDD/process framework | Evaluate only if it adds proven value |
| Capability already provided adequately by Codex | Do not reimplement |

## Separation of Concerns

Preserve these boundaries:

Codex Agent -> cognition and engineering judgment

Governance -> authority and operating rules

Repository Memory -> durable repository-specific knowledge

Skill -> reusable procedure and capability knowledge

MCP -> authenticated external capability

GitHub -> operational work state

Deterministic Tooling -> mechanical measurement, calculation, validation, and enforcement

Do not merge these responsibilities without a demonstrated reason.

## Evidence Before Custom Infrastructure

Create custom infrastructure only when evidence shows a real limitation. Before building a replacement, determine what approved requirement cannot be satisfied, why existing Codex, Skill, or MCP capabilities are insufficient, whether the limitation is reproducible, whether a thin adapter solves it, and whether maintenance and token cost are justified.

Prefer a thin compatibility layer over a new platform when possible.

## Technical Spikes

The initial runtime assumptions for isolated Agent Runs, nested delegation, per-run model and reasoning configuration, routing reclassification, Workflow Execution telemetry, and GitHub MCP operations have been validated through technical spikes. The evidence is version-sensitive and preserved under `docs/validation/`; any new or changed runtime assumption still requires validation before it is relied on.

## Initial V1 Stack

Conceptually, the initial implementation favors:

Agentic Engineering Governance
+ Codex native runtime
+ repository `AGENTS.md`
+ repository documentation
+ explicit procedure routing
+ MCP
+ thin deterministic utilities where required

Custom Skills are optional and may initially be absent.

Avoid a large custom orchestration codebase until operational evidence demonstrates the need.
