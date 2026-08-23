# Orchestration Rules

This document defines how work moves through the initial Agentic Engineering system. It does not define detailed implementation workflows, checkpoint formats, model routing, token accounting, or telemetry rules.

## Entry Point

The Root Engineering Copilot is the normal entry point for human interaction. The Human Operator should normally communicate with the Copilot rather than manually selecting an engineering agent. The Copilot determines the appropriate route from the approved requirement and existing governance.

## Technical Ownership

When work requires engineering analysis, the Lead Engineer owns the technical workflow.

The normal path is:

Human Operator -> Root Engineering Copilot -> Lead Engineer -> Specialist

The Copilot must not micromanage implementation specialists. The Lead Engineer coordinates technical execution and consolidates engineering results.

## Delegation Depth

The normal hierarchy is:

- Level 0 - Human Operator
- Level 1 - Root Engineering Copilot
- Level 2 - Lead Engineer
- Level 3 - Specialist Agent

Delegation normally stops at Level 3. Specialist agents must not recursively create additional specialist hierarchies. If a specialist discovers that another competency is required, it reports that requirement to the Lead Engineer, which decides how the additional work is routed.

## Sequential Execution

The initial version must not execute engineering specialist tasks in parallel. Only one planned engineering task may be actively executed at a time.

Even when a Technical Specification contains independent tasks, the Lead Engineer determines an execution order and processes them sequentially. Parallel execution may be considered in a future version, but is excluded from the initial version.

## Persistent Progress

Execution must never depend on conversational memory to determine where work stopped. After every completed atomic task, persistent state must identify at least:

- the parent requirement or feature;
- the technical plan;
- completed tasks;
- current task status;
- the next eligible task;
- blockers;
- pending human decisions.

A fresh Copilot or Lead Engineer session must be able to reconstruct the stopping point from persistent artifacts. The exact persistence format is intentionally undefined.

## Result Flow

Results normally move upward through the delegation hierarchy:

Specialist -> Lead Engineer -> Root Engineering Copilot -> Human Operator

A Specialist returns only the structured result and persistent artifacts required by the Lead Engineer. The Lead Engineer consolidates technical results. The Copilot receives the consolidated engineering outcome rather than every specialist's private execution history.

## Context Isolation

Private execution context must not automatically move between agents. A specialist receives only the relevant task, applicable specification, required repository context, relevant contracts, constraints, and necessary artifacts.

When execution finishes, its internal working context is not propagated to subsequent agents. Persistent artifacts and structured results are the collaboration mechanism.

## Escalation

Specialists escalate technical blockers to the Lead Engineer. The Lead Engineer escalates decisions outside its authority to the Copilot. The Copilot is the normal interface for escalating decisions to the Human Operator.

Conceptually:

Specialist -> NEEDS_DECISION -> Lead Engineer -> Copilot -> Human Operator

Specialists should not independently interrupt the Human Operator during normal execution.

## Fast Path

Not every task requires the full hierarchy.

### Administrative Work

Small administrative work may follow:

Human -> Copilot -> deterministic verification -> done

### Trivial Technical Work

A technically trivial, fully specified, low-risk repository change may follow:

Copilot -> Repository Engineer -> deterministic verification -> Copilot

This path is only appropriate when no meaningful technical design or decomposition is required.

### Normal Engineering Work

Work requiring meaningful technical analysis, design, decomposition, multiple relevant components, multiple repositories, or significant risk must use:

Copilot -> Lead Engineer -> required specialists

The Fast Path supports cost-aware execution; it does not bypass engineering governance.

## Technical Accountability

Every normal engineering workflow has one Lead Engineer responsible for the consolidated technical result. Even when multiple specialists participate sequentially, technical ownership remains with the Lead Engineer. A workflow must not reach completion without a single agent responsible for determining whether the engineering work is coherent as a whole.

## Independent Evaluation

An agent must not independently approve its own work when independent evaluation is required. Independent review uses a fresh isolated execution context.

The reviewing execution receives the required specifications, artifacts, repository rules, test evidence, and relevant diff without inheriting the implementation agent's private working context.

## Human Interaction

The Root Engineering Copilot is the normal communication boundary between the agent system and the Human Operator. The Human Operator normally receives consolidated status, important decisions, blockers, risks, final engineering results, and required manual validation.

The Human Operator should not need to follow internal conversations or intermediate reasoning of individual specialists. The Human Operator may explicitly choose to interact directly with a specialist when investigating a particular problem.

## Initial Execution Philosophy

The initial framework favors controlled sequential execution over maximum autonomous throughput. Its goals are predictable token consumption, measurable execution cost, easy interruption and resumption, clear responsibility, strong context isolation, and simple debugging of the agent system itself.

The system may become more autonomous or parallel in future versions after sufficient operational evidence has been collected.

