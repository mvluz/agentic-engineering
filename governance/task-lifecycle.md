# Atomic Task Lifecycle

This governance guideline defines the lifecycle from an approved Product Requirement through technical planning, sequential Atomic Task execution, validation, review, persistent operational state updates, and final feature completion. It does not implement integrations, automation, or storage mechanisms.

## Repository Onboarding Exception

Repository Onboarding is a narrow pre-lifecycle bootstrap for an existing repository that lacks its first local `AGENTS.md`. An explicit Human onboarding request authorizes this exception only. It uses the bounded Root Copilot -> Lead Engineer -> Repository Engineer flow to create and validate that artifact, without a Product Requirement, Technical Specification, Implementation Plan, Atomic Task, Consistency Gate, or GitHub materialization.

It must not be used for Product Work or material repository change. Those requests enter the normal lifecycle below. Detailed onboarding scope and repository isolation are governed by [governance/repository-agent-strategy.md](repository-agent-strategy.md).

## Lifecycle Levels

The engineering lifecycle has five primary levels:

Product Requirement -> Technical Specification -> Implementation Plan -> Atomic Tasks -> Implementation

The Product Requirement defines what and why. The Technical Specification defines what must be technically true. The Implementation Plan defines how the Technical Specification will be delivered. Atomic Tasks are the executable engineering units derived from the Implementation Plan. Implementation produces the resulting engineering change.

## Product Requirement

The Human Product Authority and Root Engineering Copilot define and approve the Product Requirement. It may contain:

- objective;
- business need;
- expected behavior;
- scope and out of scope;
- known constraints;
- priority;
- definition of success.

Technical implementation decisions must not be silently introduced at this stage. The Product Requirement is the upstream contract for technical engineering work. Once approved, it becomes eligible for engineering planning.

## Requirement Quality and Clarification

Before deep technical planning, meaningful ambiguity in the Product Requirement should be resolved when it would materially affect the Technical Specification. Trivial details that can be resolved safely within existing authority do not require Human clarification.

Escalation remains governed by [governance/human-gates.md](human-gates.md). This stage prevents expensive technical planning against materially ambiguous product intent and does not require a dedicated agent.

## Technical Specification

The Root Engineering Copilot routes the approved Product Requirement to the Lead Engineer. The Lead Engineer analyzes it with relevant technical evidence and repository context, then produces:

- the Technical Specification;
- relevant architecture and system behavior;
- technical constraints and interfaces;
- API contracts and data requirements;
- integration boundaries and security requirements;
- technical acceptance conditions and compatibility expectations.

The Technical Specification is the technical contract that implementation must satisfy. It answers: “What must be technically true when this requirement is correctly implemented?”

## Implementation Plan

The Lead Engineer creates the Implementation Plan from the approved Technical Specification. It defines how the specification will be delivered and may include affected repositories and components, implementation approach, dependency order, migration strategy, integration sequence, validation strategy, required Specialists, technical risks, and execution ordering.

The Implementation Plan must remain consistent with the Technical Specification. If it requires behavior that contradicts or exceeds the specification, the specification must be reconsidered rather than silently bypassed.

The Lead Engineer owns both technical artifacts. The Copilot must not author the technical solution on behalf of the Lead Engineer.

## Sequential Task Plan

The initial version executes Atomic Tasks sequentially. Only one planned engineering task is actively executed at a time, and the Lead Engineer determines the order. Parallel specialist implementation is excluded from the initial version.

## Atomic Tasks

Atomic Tasks are derived from the Implementation Plan and must not be generated independently of the approved Plan. The relationship is:

Product Requirement -> Technical Specification -> Implementation Plan -> Atomic Tasks -> Implementation

Traceability between these levels must be preserved.

## Atomic Task Readiness

Before execution, an Atomic Task must contain sufficient information for isolated work, including when applicable:

- unique task identifier;
- objective;
- relevant technical context;
- scope and out of scope;
- acceptance criteria;
- dependencies;
- relevant contracts or artifacts;
- execution configuration is selected by orchestration and normally remains separate from task content.

An Atomic Task should be independently understandable without requiring reconstruction of the complete parent-workflow history.

## Context Routing

The Lead Engineer provides the executing specialist only the context necessary for the current Atomic Task, such as the task, applicable Technical Specification, repository instructions, relevant contracts, project documentation, required artifacts, and known constraints.

Complete conversation history, private reasoning from previous agents, unrelated future tasks, and unnecessary project context are not automatically propagated. Persistent artifacts and repository knowledge are the durable mechanisms for transferring technical context.

## Consistency Gate

After Atomic Tasks are generated and before implementation begins, a read-only Consistency Gate evaluates whether the Technical Specification, Implementation Plan, and Atomic Tasks are coherent. It checks whether specification requirements are represented in the Plan, required Plan activities are represented by tasks, dependencies and acceptance conditions are covered, and material contradictions or gaps are absent.

The Consistency Gate must not silently rewrite the Technical Specification, Implementation Plan, or Atomic Tasks. Its preferred successful result is `PASS`; inconsistencies are returned as concise actionable findings under [governance/agent-contracts.md](agent-contracts.md).

No permanent agent role is created for this gate. It may initially use a fresh isolated Lead Engineer evaluation or another approved independent execution, evaluating persistent artifacts without inheriting private planning context.

## Publish Operational Tasks

After the Consistency Gate passes, Atomic Tasks become eligible for representation in the intended operational source of truth: GitHub Issues / GitHub Projects.

GitHub operational materialization follows [governance/github-workflow.md](github-workflow.md). GitHub MCP is the validated operational capability; concrete automation and command implementation remain outside this lifecycle policy.

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

Quality Engineer involvement is on-demand. The Human Operator decides whether to request it after implementation is technically ready, as governed by [governance/testing-strategy.md](testing-strategy.md). When invoked, it evaluates whether sufficient evidence shows that the implementation satisfies the specification and acceptance criteria.

The Quality Engineer may review acceptance coverage, inspect tests, identify missing scenarios and edge cases, add or request tests when appropriate, and produce a concise manual verification checklist when the Human requests one. It must not fix production implementation code. Implementation defects return through the Lead Engineer to the responsible Repository Engineer.

## Manual Verification

Manual verification may be used when automated verification would create disproportionate cost or complexity, particularly for visual frontend or mobile behavior during the initial phase. When required, the Quality Engineer provides a concise and reproducible checklist. Manual verification is conditional and is not required for every Atomic Task.

## Independent Code Review

When required, Code Review uses a fresh isolated execution. The reviewer receives only necessary evidence, such as the Atomic Task, relevant Technical Specification, acceptance criteria, repository rules, relevant diff, tests, and deterministic verification evidence.

Possible outcomes include `PASS` and `CHANGES_REQUIRED`. When changes are required, the path is:

Code Reviewer -> Lead Engineer -> Repository Engineer -> deterministic verification -> required evaluation again

The implementation agent must not independently approve its own work.

## Conditional Specialist Evaluation

Security, architecture, UX/UI, and other specialist evaluation is conditional. Security-sensitive work may require Security Reviewer evaluation, major architectural changes may require independent architecture evaluation, and interface design may require UX/UI Designer involvement.

The Lead Engineer determines required specialist involvement according to the Implementation Plan and governance. These are not mandatory steps for every Atomic Task.

## Correction Loops

Validation, Quality Engineering, Code Review, or specialist evaluation may return work for correction:

Specialist implementation -> validation -> evaluation -> issue found -> Lead Engineer -> implementing specialist -> correction -> validation again

Correction loops must remain bounded. Retry limits, escalation thresholds, and termination conditions are defined separately under Human Gates and Termination Conditions governance and are not defined here.

## Technical Completion

An Atomic Task becomes technically complete only when all required lifecycle stages have succeeded. Depending on the task, this may include implementation, deterministic checks, required Quality Engineering, Code Review, Security or Architecture Review, and manual verification.

The Lead Engineer determines whether the Atomic Task satisfies the Technical Specification as a whole. The resulting status is `TECHNICALLY_COMPLETE`.

## Operational Work State

Operational progress must not depend on Markdown checklists or repository files used as temporary task trackers. The intended source of truth is the external work-management system, initially GitHub Issues / GitHub Projects.

The validated GitHub MCP capability is used primarily by the Root Engineering Copilot under [governance/github-mcp.md](github-mcp.md). The approved representation is defined in [governance/github-workflow.md](github-workflow.md); this document does not implement it.

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

GitHub fields, hierarchy, and Project configuration are defined in [governance/github-workflow.md](github-workflow.md). MCP command and automation implementation remain outside this document.

## Mandatory Stop Between Atomic Tasks

Every Atomic Task Agent Run ends at a mandatory boundary:

Atomic Task -> technical completion -> operational state update -> STOP

The next Atomic Task may be identified as ready, but may start only after the prior Task's final operational state is persisted. The Root Engineering Copilot may continue when the current Human authorization covers the containing User Story or Feature. If only one Task was authorized, execution remains stopped for Human review. Mandatory Human Gates, blockers, and retry limits still stop continuation.

This preserves explicit control over token consumption, predictable execution cost, easy interruption and resumption, and controlled observation of the initial framework without requiring redundant approval between Tasks already covered by the current Human authorization.

## Session Resumption

A fresh Copilot or Lead Engineer session must reconstruct progress from persistent sources of truth. Conversational memory must not be required to determine the active requirement, Technical Specification, Implementation Plan, completed task, next task, blockers, or pending human decision.

In the intended architecture, operational progress is primarily reconstructed from GitHub Issues / Projects, while durable technical knowledge is reconstructed from repository artifacts.

## Convergence Check

After all planned Atomic Tasks are technically complete and before final Lead consolidation and Human Final Engineering Review, a fresh isolated Convergence Check compares the final implementation with the approved Product Requirement, Technical Specification, and Implementation Plan.

It uses current implementation evidence to determine whether required behavior is present, the specification is satisfied, the Plan was realized where applicable, required validation exists, and implementation gaps or omitted work remain. It must not rely on task completion statuses alone or inherit implementation agents' private conversations.

Its preferred result is `PASS`. If gaps remain, it returns `GAPS_FOUND` followed only by concise actionable findings. Real remaining work returns to the Lead Engineer, which determines required changes, creates or modifies Atomic Tasks as appropriate, updates operational state, and resumes sequential execution. No dedicated permanent agent role is created solely for convergence.

## Delivery-Boundary Completion

The Human-selected delivery and validation boundary may be one Task, a User Story, or a Feature. When the authorized boundary contains multiple Tasks, each Task completes and persists sequentially before the next begins. Completing an internal Task does not require a new Human approval while the larger authorized scope still permits continuation.

When the delivery unit's technical work has converged, including the Convergence Check when applicable, the Lead Engineer determines it is `TECHNICALLY_COMPLETE` and returns the consolidated result to the Root Engineering Copilot. The delivery unit becomes `DONE` only after the required Human review or approval. For a single authorized Task, this review occurs after that Task's deterministic validation and technical completion.

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

The human-facing GitHub Project Status mapping is defined in [governance/github-workflow.md](github-workflow.md).

## Initial Lifecycle Summary

Conceptually:

Human + Copilot -> Product Requirement -> Requirement Quality / Clarification -> Lead Engineer -> Technical Specification -> Implementation Plan -> Atomic Tasks -> Consistency Gate -> publish operational work to GitHub -> authorize Task, User Story, or Feature -> sequential Repository Engineer Task runs -> deterministic verification -> required independent evaluation -> correction loop when necessary -> persist each Task state -> STOP boundary -> continue only within current authorized scope

When all Atomic Tasks are complete:

Authorized delivery unit converges -> `TECHNICALLY_COMPLETE` -> Root Engineering Copilot -> Human review or approval -> `DONE`

Agentic Engineering owns these lifecycle concepts independently. Spec Kit is not a dependency, and no Spec Kit commands or file conventions are introduced here.
