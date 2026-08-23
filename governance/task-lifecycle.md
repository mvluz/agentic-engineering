# Atomic Task Lifecycle

This governance guideline defines the lifecycle from an approved Product Requirement through technical planning, sequential Atomic Task execution, validation, review, persistent operational state updates, and final feature completion. It does not implement integrations, automation, or storage mechanisms.

## Lifecycle Levels

The engineering lifecycle has three primary levels:

Product Requirement -> Technical Plan -> Atomic Tasks

The Product Requirement defines what and why. The Technical Plan defines how the approved requirement will be implemented. Atomic Tasks are the individual executable engineering units produced from the Technical Plan.

## Product Requirement

The Human Product Authority and Root Engineering Copilot define and approve the Product Requirement. It may contain:

- objective;
- business need;
- expected behavior;
- scope and out of scope;
- known constraints;
- priority;
- definition of success.

Technical implementation decisions must not be silently introduced at this stage. Once approved, the requirement becomes eligible for engineering planning.

## Engineering Planning

The Root Engineering Copilot routes the approved Product Requirement to the Lead Engineer. The Lead Engineer analyzes it with relevant technical evidence and repository context, then produces:

- the Technical Specification;
- the ordered Atomic Task Plan;
- relevant dependencies;
- required specialist involvement;
- execution classification when applicable.

The Lead Engineer owns the technical plan. The Copilot must not author the technical solution on behalf of the Lead Engineer.

## Sequential Task Plan

The initial version executes Atomic Tasks sequentially. Only one planned engineering task is actively executed at a time, and the Lead Engineer determines the order. Parallel specialist implementation is excluded from the initial version.

## Atomic Task Readiness

Before execution, an Atomic Task must contain sufficient information for isolated work, including when applicable:

- unique task identifier;
- objective;
- relevant technical context;
- scope and out of scope;
- acceptance criteria;
- dependencies;
- relevant contracts or artifacts;
- execution class.

An Atomic Task should be independently understandable without requiring reconstruction of the complete parent-workflow history.

## Context Routing

The Lead Engineer provides the executing specialist only the context necessary for the current Atomic Task, such as the task, applicable Technical Specification, repository instructions, relevant contracts, project documentation, required artifacts, and known constraints.

Complete conversation history, private reasoning from previous agents, unrelated future tasks, and unnecessary project context are not automatically propagated. Persistent artifacts and repository knowledge are the durable mechanisms for transferring technical context.

## Implementation

The appropriate Repository Engineer executes the Atomic Task and normally:

- implements the requested change;
- creates or updates relevant tests;
- runs applicable deterministic validation;
- updates technical artifacts when required;
- reports implementation evidence and blockers.

Implementation remains within the approved Atomic Task scope.

## Deterministic Verification First

Before additional reasoning-heavy evaluation, applicable deterministic verification is run. Depending on the repository, this may include builds, linting, formatting checks, unit tests, integration tests, schema validation, static checks, or project-specific verification commands.

Failures normally return to the implementing Repository Engineer for correction before independent evaluation continues.

## Quality Evaluation

When required by the Technical Plan or governance, a Quality Engineer evaluates whether sufficient evidence shows that the implementation satisfies the specification and acceptance criteria.

The Quality Engineer may review acceptance coverage, inspect tests, identify missing scenarios and edge cases, add or request tests when appropriate, and produce a concise manual verification checklist when automation is not cost-effective. It must not fix production implementation code. Implementation defects return through the Lead Engineer to the responsible Repository Engineer.

## Manual Verification

Manual verification may be used when automated verification would create disproportionate cost or complexity, particularly for visual frontend or mobile behavior during the initial phase. When required, the Quality Engineer provides a concise and reproducible checklist. Manual verification is conditional and is not required for every Atomic Task.

## Independent Code Review

When required, Code Review uses a fresh isolated execution. The reviewer receives only necessary evidence, such as the Atomic Task, relevant Technical Specification, acceptance criteria, repository rules, relevant diff, tests, and deterministic verification evidence.

Possible outcomes include `PASS` and `CHANGES_REQUIRED`. When changes are required, the path is:

Code Reviewer -> Lead Engineer -> Repository Engineer -> deterministic verification -> required evaluation again

The implementation agent must not independently approve its own work.

## Conditional Specialist Evaluation

Security, architecture, UX/UI, and other specialist evaluation is conditional. Security-sensitive work may require Security Reviewer evaluation, major architectural changes may require independent architecture evaluation, and interface design may require UX/UI Designer involvement.

The Lead Engineer determines required specialist involvement according to the Technical Plan and governance. These are not mandatory steps for every Atomic Task.

## Correction Loops

Validation, Quality Engineering, Code Review, or specialist evaluation may return work for correction:

Specialist implementation -> validation -> evaluation -> issue found -> Lead Engineer -> implementing specialist -> correction -> validation again

Correction loops must remain bounded. Retry limits, escalation thresholds, and termination conditions are defined separately under Human Gates and Termination Conditions governance and are not defined here.

## Technical Completion

An Atomic Task becomes technically complete only when all required lifecycle stages have succeeded. Depending on the task, this may include implementation, deterministic checks, required Quality Engineering, Code Review, Security or Architecture Review, and manual verification.

The Lead Engineer determines whether the Atomic Task satisfies the Technical Specification as a whole. The resulting status is `TECHNICALLY_COMPLETE`.

## Operational Work State

Operational progress must not depend on Markdown checklists or repository files used as temporary task trackers. The intended source of truth is the external work-management system, initially GitHub Issues / GitHub Projects.

This integration will later be accessed through approved GitHub tooling or MCP capabilities. GitHub integration is not implemented yet and must not be assumed to exist. This document defines intended behavior only.

## Repository Memory vs Operational State

### Repository Memory

Durable technical knowledge belongs close to the code. Examples include `AGENTS.md`, repository-specific instructions, architecture and testing documentation, API contracts, coding standards, ADRs, and durable Technical Specifications where appropriate.

Repository memory answers: “How does this repository work and how should an agent work with it?”

### Operational Work State

Operational state describes current execution progress, including the current and completed tasks, next task, blockers, pending decisions, task status, and references to commits, pull requests, or artifacts.

Operational work state answers: “What work is currently happening and where did execution stop?” It belongs in the designated external work-management system rather than temporary repository tracking files.

## State Update After Every Atomic Task

After an Atomic Task reaches technical completion, its operational state is persisted before any following Atomic Task begins. When applicable, persistent state identifies:

- which Atomic Task completed;
- its final status;
- implementation evidence;
- blockers;
- pending decisions;
- references to commits, pull requests, or artifacts;
- the next eligible Atomic Task.

The exact GitHub fields, Project configuration, Issue structure, and MCP commands are intentionally undefined.

## Mandatory Stop Between Atomic Tasks

Completion of one Atomic Task does not automatically authorize the next. The initial lifecycle is:

Atomic Task -> technical completion -> operational state update -> STOP

The next Atomic Task may be identified as ready, but a new execution requires Human Operator authorization through the Root Engineering Copilot. This provides explicit control over token consumption, predictable execution cost, easy interruption and resumption, and controlled observation of the initial framework.

Future versions may introduce bounded automatic continuation, but it is disabled in the initial version.

## Session Resumption

A fresh Copilot or Lead Engineer session must reconstruct progress from persistent sources of truth. Conversational memory must not be required to determine the active requirement, Technical Plan, completed task, next task, blockers, or pending human decision.

In the intended architecture, operational progress is primarily reconstructed from GitHub Issues / Projects, while durable technical knowledge is reconstructed from repository artifacts.

## Feature Completion

When all Atomic Tasks in a Technical Plan are technically complete, the Lead Engineer performs final technical consolidation of the feature or requirement, including when applicable implementation outcome, completed tasks, validation evidence, reviews, known risks, remaining limitations, and relevant technical artifacts.

The Lead Engineer returns this consolidated result to the Root Engineering Copilot. The Copilot presents it to the Human Operator, who acts as the Final Engineering Authority. Only after this final review may the overall feature or requirement be considered complete.

## Initial Lifecycle States

The initial conceptual state vocabulary is intentionally small:

- `PLANNED`;
- `READY`;
- `IN_PROGRESS`;
- `BLOCKED`;
- `NEEDS_DECISION`;
- `VALIDATING`;
- `CHANGES_REQUIRED`;
- `TECHNICALLY_COMPLETE`;
- `DONE`.

The precise mapping of these states to GitHub Issues or GitHub Projects is intentionally undefined.

## Initial Lifecycle Summary

Conceptually:

Human + Copilot -> Product Requirement -> Lead Engineer -> Technical Specification -> Ordered Atomic Tasks -> current task READY -> Repository Engineer -> deterministic verification -> required independent evaluation -> correction loop when necessary -> TECHNICALLY_COMPLETE -> persist operational state -> STOP -> Human authorizes next Atomic Task

When all Atomic Tasks are complete:

Lead Engineer -> final technical consolidation -> Root Engineering Copilot -> Human Final Engineering Review -> DONE

