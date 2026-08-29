# Execution State and Checkpoints

This document defines the logical Execution State and Checkpoint strategy for the initial Agentic Engineering system. It defines what operational state must survive isolated executions and fresh sessions, not how that state is stored.

## Core Principle

A checkpoint must preserve enough operational state to resume safely, but no more than is necessary to determine current position, dependencies, blockers, and next eligible work.

Operational state must never depend on conversational memory.

## Operational Source of Truth

The intended operational source of truth is GitHub Issues / GitHub Projects. GitHub MCP has passed its initial technical validation and is the intended operational capability. The detailed GitHub workflow remains future work.

The distinction is:

- GitHub: operational work state;
- repository: durable technical memory and engineering artifacts;
- conversation: temporary execution context, never authoritative state.

Repository files must not become competing temporary task-progress trackers.

## When Operational State Begins

Operational execution state begins after planning produces:

Product Requirement -> Technical Specification -> Implementation Plan -> Atomic Tasks -> Consistency Gate PASS

After the Consistency Gate succeeds, approved Atomic Tasks become eligible to be materialized as GitHub operational work. Their exact representation is defined later.

## Minimum Checkpoint Contract

When applicable, a checkpoint must preserve enough information to determine:

- active Product Requirement or Feature and relevant repository or repositories;
- references to the Technical Specification, Implementation Plan, and other required durable artifacts;
- last completed Atomic Task, current Atomic Task, next eligible Atomic Task, and current operational status;
- blockers, pending Human decisions, replan requirements, and unresolved dependencies;
- relevant commits, Pull Requests, implementation artifacts, validation status, and review status.

Checkpoints should reference authoritative artifacts rather than duplicate their contents.

## Initial Operational States

The initial lifecycle vocabulary is intentionally small:

- `PLANNED`
- `READY`
- `IN_PROGRESS`
- `BLOCKED`
- `NEEDS_DECISION`
- `VALIDATING`
- `CHANGES_REQUIRED`
- `TECHNICALLY_COMPLETE`
- `DONE`

No additional states should be introduced unless a later GitHub Workflow design demonstrates a real requirement.

Meaningful transitions include:

Atomic Task created -> `READY`

Execution authorized -> `IN_PROGRESS`

Blocker discovered -> `BLOCKED`

Human decision required -> `NEEDS_DECISION`

Required validation begins -> `VALIDATING`

Evaluation requires correction -> `CHANGES_REQUIRED`

Technical lifecycle requirements succeed -> `TECHNICALLY_COMPLETE`

Operational evidence is persisted -> `DONE`

After `DONE`, identify the next eligible Atomic Task and STOP. The next task must not start automatically.

## State Update Responsibility

The approved hierarchy remains:

- Repository Engineer reports implementation results and evidence;
- Quality Engineer and Reviewer report evaluation results and evidence;
- Lead Engineer determines the consolidated technical state;
- Root Engineering Copilot persists operational state in GitHub.

Specialists and the Lead Engineer do not require direct GitHub MCP access merely to report routine progress. The Root Engineering Copilot remains the primary GitHub operational capability owner in v1.

Operational state is a current-state checkpoint, not an execution diary. Do not record every inspected file, hypothesis, reasoning step, tool invocation, or temporary implementation attempt. Git and Pull Requests preserve implementation changes, Execution Telemetry preserves runtime measurements, repository artifacts preserve technical knowledge, and Agent Contracts preserve concise execution outcomes.

## Mandatory Persistence Boundary

No next Atomic Task may start until the previous task's final operational state has been persisted successfully.

Atomic Task -> technically complete -> persist evidence and final state -> `DONE` -> identify next eligible task -> STOP

If operational state cannot be persisted safely, do not begin the next Atomic Task. Report the persistence problem through the normal hierarchy.

## Fresh Session Reconstruction

A fresh Root Engineering Copilot or Lead Engineer session must reconstruct current execution position without previous conversation history:

Fresh session -> query GitHub operational state -> identify active work -> determine current or last completed task -> determine next eligible task -> identify blockers and decisions -> resolve specification and plan references -> retrieve repository context when required

It must be possible to determine what is being worked on, the last completed task, whether work is in progress, the next eligible task, blockers, pending Human decisions, and the locations of the Technical Specification and Implementation Plan.

## Interrupted Execution and State Reconciliation

`IN_PROGRESS` does not prove completion. If execution terminates unexpectedly while an Atomic Task is `IN_PROGRESS`, a fresh session must not assume success, mark the task `DONE` without evidence, or start the next task automatically.

State Reconciliation determines whether operational state and technical evidence remain consistent. Relevant evidence may include repository state, commits, Pull Requests, implementation artifacts, validation results, structured handoffs, and current GitHub state.

Reconciliation may result in:

- Resume: the task remains incomplete and continues from durable evidence;
- Restart: insufficient durable progress requires executing the task again;
- Reconcile Completion: objective evidence proves completion and operational state is updated;
- Block: evidence is insufficient or contradictory and requires diagnosis or decision.

State Reconciliation must not reconstruct private reasoning from the interrupted agent.

## Replanning

When the Lead Engineer determines that the approved Implementation Plan must change, `REPLAN_REQUIRED` must be reflected in the operational workflow as appropriate. Affected Atomic Tasks may become obsolete, superseded, modified, or replaced.

After replanning:

1. update the Implementation Plan;
2. derive or update Atomic Tasks;
3. perform required consistency validation;
4. update operational work;
5. resume sequential execution.

Do not silently execute tasks derived from an invalidated Plan. The exact representation of superseded or replaced tasks is deferred.

## Technical and Operational Completion

`TECHNICALLY_COMPLETE` and `DONE` are distinct. The former means the Lead Engineer determined that required technical lifecycle conditions succeeded. The latter means the required operational evidence and state were also persisted successfully.

Technical lifecycle complete -> `TECHNICALLY_COMPLETE` -> persist operational evidence -> `DONE`

## Blockers, Decisions, and Evidence

Blockers and pending decisions that prevent safe continuation must be preserved concisely, for example:

BLOCKED

Reason:
<concise blocker>

or:

NEEDS_DECISION

Decision:
<required Human decision reference>

Checkpoint state should reference authoritative Technical Specifications, Implementation Plans, commits, Pull Requests, validation evidence, and review evidence. Follow: "One authoritative home whenever practical."

## Relationship With GitHub Workflow

This document defines what operational state must survive. Future GitHub Workflow governance will define how that state is represented using GitHub Projects, Issues, relationships, statuses, labels where appropriate, Pull Request references, and MCP operations.

It does not define Project fields, Issue templates, Issue hierarchy, labels, status mappings, Feature-to-Issue representation, concrete MCP commands, schemas, synchronization automation, or automatic recovery mechanisms.

## Relationship With Existing Governance

Execution State must remain consistent with:

- `governance/task-lifecycle.md`;
- `governance/orchestration.md`;
- `governance/context-artifact-strategy.md`;
- `governance/agent-contracts.md`;
- `governance/github-mcp.md`;
- `governance/human-gates.md`.

Detailed policies remain owned by those documents.

## Initial Strategy Summary

1. GitHub Issues / Projects are the operational source of truth.
2. Repository artifacts are technical memory, not task-progress state.
3. Conversational memory is never authoritative.
4. Persist only the state necessary for safe continuation.
5. Update state at meaningful lifecycle transitions.
6. The Copilot owns routine operational persistence in v1.
7. No next Atomic Task begins before previous final state is persisted.
8. `IN_PROGRESS` never implies completion.
9. Interrupted work requires State Reconciliation.
10. Replanning invalidates affected downstream execution state.
11. `TECHNICALLY_COMPLETE` and `DONE` are distinct.
12. Fresh sessions reconstruct state from persistent sources.
