# Context and Artifact Strategy

This architectural policy defines how execution context is assembled, durable knowledge is preserved, operational work state is separated from repository memory, and agents collaborate without inheriting unnecessary conversational context. It does not implement tooling, MCP, context loaders, schemas, or storage mechanisms.

## Core Principle

Context is assembled for the current execution, not inherited from the previous conversation.

Each Agent Run receives the minimum context necessary to perform its current responsibility correctly. Private execution context must not automatically propagate between runs.

## Information Categories

### Governance

Global rules defining how Agentic Engineering operates, including principles, orchestration, model routing, human gates, agent contracts, and context rules. Governance answers: “How must the agent system operate?”

An agent receives only governance applicable to its current execution; it must not automatically load every governance document.

### Repository Memory

Durable technical knowledge kept close to the code, including `AGENTS.md`, repository-specific guidance, architecture and testing documentation, coding standards, API contracts, ADRs, durable Technical Specifications, and project conventions.

Repository Memory answers: “How does this repository work and how should work be performed here?” It must not be used as a temporary task-progress tracker.

### Operational Work State

Information about current progress, including the active Product Requirement, current Atomic Task, task status, completed tasks, next eligible task, blockers, pending decisions, pull requests, and relevant commits.

Operational Work State answers: “Where is the work currently stopped?” The intended source of truth is GitHub Issues / GitHub Projects. GitHub MCP is the validated authenticated capability for this operational state; its approved workflow is defined in [governance/github-workflow.md](github-workflow.md).

## Execution Context Pack

An **Execution Context Pack** is the logical minimum set of information and artifact references assembled for one isolated Agent Run. It is not necessarily a physical file.

When applicable, it contains:

- task identifier, role responsibility, objective, and acceptance criteria;
- scope, out of scope, applicable constraints, and allowed capabilities;
- relevant Technical Specification section, repository instructions, contracts, architecture information, and project documentation;
- references to required artifacts, implementation evidence, and prior useful findings;
- expected output contract, required evidence, and required artifact references.

Information is not included merely because it is available.

### Separate Orchestration Metadata

Execution configuration is primarily orchestration/runtime metadata, including execution class, selected model, reasoning effort, and runtime configuration. The calling orchestration layer uses it to create the Agent Run.

These values do not normally belong in a Specialist's Execution Context Pack. Conceptually:

Lead Engineer -> determines Specialist configuration -> launches Specialist -> Specialist receives required task context

Root Engineering Copilot -> determines Lead configuration -> launches Lead Engineer

Least Context applies to orchestration metadata as well.

## Targeted Retrieval and Just-in-Time Context

Prefer reference + targeted retrieval over embedding complete artifacts into every handoff. Reference the relevant Technical Specification and retrieve its required section instead of copying a large specification into every run.

Do not preload large amounts of context merely because it might become useful:

minimum sufficient context -> legitimate information need -> targeted retrieval -> continue

Agents retrieve additional context only when relevant to the current task.

### Conditional and Targeted Retrieval

Just-in-Time retrieval is conditional. Retrieve a governance document, repository artifact, or additional evidence only when the current decision, explicit contract, ambiguity, gate, security concern, routing question, or testing condition requires it. Role applicability alone is not sufficient reason to read a complete document.

Prefer a relevant heading, excerpt, reference, or bounded search over a complete large file or broad collection. A complete read remains appropriate when the whole document is small, the whole document is needed, or its internal relationships materially affect the decision. Do not repeat a read when its result remains active in the execution or is available in a concise handoff; repeat it only when the source changed, the prior read was incomplete, exact verification is required, or an independent evaluation needs to inspect the evidence itself.

### Bounded Discovery and Execution

Start unfamiliar repository work with bounded deterministic discovery: repository root, Git status, concise top-level structure, build or package metadata, and documentation references. Expand to source, generated output, dependency directories, large artifacts, or broad searches only to answer a concrete current question. Bound tool output and avoid unrestricted recursion or repeated large inspections.

Batch safe low-risk discovery where practical. Model calls, delegations, and intermediate narration must serve a clear decision or required outcome; agents should not accumulate context through acknowledgements, status narration, or speculative exploration.

## Artifact-Based Continuity

Continuity between Agent Runs comes from durable artifacts and structured handoffs, not private conversational memory:

Agent Run A -> useful artifacts and structured outcome -> run ends

Agent Run B -> current task, relevant artifacts and findings -> fresh isolated context

Run A's private reasoning and complete conversation are not propagated.

### Parent-to-Child Context

A parent provides a child the current objective, acceptance criteria, scope, authority, target repository, resolved decisions, exact authoritative references, relevant evidence, and expected output. It passes conclusions and references rather than raw exploratory tool output, duplicated artifacts, or execution narrative. The child retrieves further detail Just-in-Time.

Execution configuration remains orchestration metadata. A child executes under its assigned configuration and must not reread Model Routing merely to infer that configuration. When independent verification is required, the child validates the relevant claim from objective evidence rather than trusting an unsupported parent conclusion.

### Reclassification Continuity

When a run ends with `NEEDS_RECLASSIFICATION`, the routing authority may start a new run with different execution configuration. The new run continues through the original task, current relevant specification, persistent artifacts, useful findings, evidence, and concise handoff information. It must not depend on the previous run's private context.

## Artifact Categories

Use flexible categories rather than a rigid inventory:

- **Intent Artifacts:** Product Requirement and acceptance criteria.
- **Technical Artifacts:** Technical Specification, architecture decisions, API contracts, schemas, and migration plans.
- **Implementation Artifacts:** source code, tests, migrations, and configuration.
- **Verification Artifacts:** deterministic test results, build evidence, review findings, Quality Engineering findings, and manual verification checklists.
- **Operational References:** GitHub Issues, Project items, pull requests, and commits.

Operational references point to the work-management system and should not be duplicated unnecessarily as repository progress files.

## Artifact Ownership

Authoritative responsibility is clear:

- Root Engineering Copilot / Product layer: Product Requirement coordination;
- Lead Engineer: Technical Specification and Technical Plan;
- Repository Engineer: implementation artifacts;
- Quality Engineer: quality evidence and manual verification guidance;
- Reviewer: review findings;
- GitHub Issues / Projects: Operational Work State.

Multiple agents should not maintain competing authoritative versions of the same durable fact.

## One Authoritative Home

A durable fact should have one authoritative home whenever practical. Avoid duplicating information across files, Issues, prompts, and documents; reference the authoritative artifact instead.

For example, an Atomic Task references the authoritative API contract rather than copying the entire contract unless isolated execution requires it.

## Source-of-Truth Precedence

When information conflicts, agents must not silently choose a convenient source. When applicable, precedence is:

1. current explicit Human instruction;
2. approved active Product Requirement and Technical Specification;
3. applicable governance;
4. current repository instructions and authoritative contracts;
5. current repository implementation and deterministic evidence;
6. historical artifacts;
7. conversational memory.

This precedence does not authorize bypassing mandatory security or governance constraints. Material unresolved conflicts are reported and escalated through the normal hierarchy. Conversational memory is never authoritative engineering state.

## Stale or Conflicting Artifacts

Persistent documentation may become stale. When meaningful conflict exists between documentation, contracts, implementation, specification, or deterministic evidence, agents must not guess. They report the inconsistency so the responsible authority can determine which source must be corrected.

## Private Context Lifetime

Private execution context exists only for the current Agent Run. Private reasoning, discarded hypotheses, exploratory thoughts, irrelevant investigation paths, and unnecessary narration do not automatically survive.

Only useful durable information survives through artifacts, decisions, evidence, findings, blockers, and required actions.

## Independent Evaluation Context

Independent Quality Engineering and Code Review must not inherit an implementation agent's private conversation or subjective justification. They receive only the objective evidence needed for the evaluation: task scope, relevant specification excerpts, acceptance criteria, applicable repository rules, implementation artifacts or diff, tests, and deterministic validation evidence. They do not reload the complete repository or governance corpus unless a concrete finding requires it.

This preserves independence and reduces confirmation bias.

## Copilot Context

The Root Engineering Copilot normally operates with a smaller consolidated context than technical Specialists. It generally needs the current requirement, operational status, important decisions, blockers, risks, consolidated technical results, next eligible work, Human Gates, and telemetry summaries when available.

The Copilot should not routinely load implementation code or Specialist execution histories. Detailed technical context is retrieved only when required.

## Fresh Session Reconstruction

A fresh Copilot or Lead Engineer session must reconstruct relevant state without prior conversational memory:

Fresh session -> applicable governance -> operational state from GitHub -> relevant repository memory -> durable artifacts -> continue work

The intended system should allow a fresh session to determine the active requirement, current or last completed task, next eligible task, blockers, required decisions, and relevant technical artifacts. GitHub operational representation is governed by [governance/github-workflow.md](github-workflow.md); implementation mechanics remain separate.

## Token Efficiency

Context management is part of cost-aware engineering. Reduce unnecessary consumption by avoiding repeated large transfers, referencing artifacts rather than copying them, retrieving information only when needed, not forwarding private conversations, keeping Copilot context consolidated, and avoiding duplicated sources of truth. Execution telemetry distinguishes total input, cached input, and output; cached input may be priced differently, but repeated cached processing is still unnecessary context consumption.

More context is not automatically better context. The preferred context is the smallest reliable context that enables correct execution.

## Initial Strategy Summary

1. Context is assembled, not inherited.
2. Give each agent only what it needs.
3. Prefer references and targeted retrieval.
4. Use Just-in-Time Context.
5. Durable knowledge survives through artifacts.
6. Operational progress belongs in GitHub Issues / Projects.
7. Private execution context dies with the Agent Run.
8. Independent evaluators receive objective artifacts, not developer narratives.
9. Do not duplicate authoritative information unnecessarily.
10. Fresh sessions reconstruct state from persistent sources.

## Deferred Implementation Decisions

This document does not decide the final specification directory structure, physical Execution Context Pack format, JSON/YAML schemas, MCP retrieval commands, automatic context injection, maximum context-token budgets, caching, automatic summaries, GitHub Project fields, GitHub Issue templates, or storage implementation.
