# Human Gates, Escalation, and Termination

This policy defines when agents may continue autonomously, when work must stop, and when control moves upward. It does not implement workflow automation or tooling.

## Core Principle

Agent autonomy exists only within approved product scope, approved technical direction, assigned execution authority, approved model-routing policy, bounded correction attempts, and applicable governance. Agents must not silently expand their authority when they encounter uncertainty.

## Escalation Hierarchy

Normal escalation follows:

Specialist -> Lead Engineer -> Root Engineering Copilot -> Human Operator

A Specialist first attempts resolution using the approved task, specification, repository memory, contracts, code, and available evidence. Issues outside its authority go to the Lead Engineer. The Lead Engineer resolves technical decisions within its authority; only decisions outside that authority normally reach the Copilot and, when required, the Human Operator. Routine technical decisions should not interrupt the Human Operator.

## Human Gates

Human approval is required when execution would materially change or exceed previously approved authority.

### Product and Scope

Require approval for meaningful Product Requirement changes, significant scope expansion, removal of behavior required by the approved requirement, materially different product behavior, or decisions that redefine the intended outcome.

### Architecture

Require approval for fundamental architectural changes, adoption of a new foundational technology, changes to major system boundaries, or important cross-repository architecture changes. Normal implementation decisions within established architecture remain under Lead Engineer authority.

### Security

Require approval when a proposed change materially changes security posture, including new exposure or attack surface, material authentication or authorization changes, changed credential or secret handling, reduced security controls, or meaningful privileged access.

Credentials must never enter agent context, repository artifacts, logs, or prompts.

### Significant Cost

Model Routing and bounded execution normally control cost. Work that materially exceeds its expected execution budget or requires cost escalation beyond approved governance limits must escalate for a Human decision. Numeric budgets and thresholds are defined separately by the Execution Budget Policy.

### Destructive or Irreversible Actions

Approval is required before significant destructive or difficult-to-reverse operations, including deleting important data, destroying infrastructure, deleting production databases or important remote resources, force pushing, rewriting shared Git history, or other materially irreversible actions. Agents may analyze or prepare such actions without executing them.

## Non-Blocking Notifications

Informational notifications are distinct from Human Gates and do not stop authorized execution. Any execution using GPT-5.6 Sol, regardless of reasoning level, must generate a non-blocking notification through the Root Engineering Copilot to the Human Operator. The notification does not require approval and must not stop execution.

Detailed model-routing behavior belongs in [governance/model-routing.md](model-routing.md); this document does not implement notifications.

## Bounded Correction Loops

Agents must not enter unbounded implementation, validation, Quality Engineering, or Code Review loops. The initial version allows at most two correction cycles for the same unresolved problem before control returns to the Lead Engineer:

initial attempt -> failure -> correction 1 -> failure -> correction 2 -> same problem remains -> STOP -> Lead Engineer diagnosis

This limit applies to repeated correction of the same underlying problem, not to the total number of corrections in an entire feature.

## Lead Diagnosis After Retry Limit

Reaching the retry limit triggers diagnosis rather than another automatic retry. The Lead Engineer should determine whether the cause is an incorrect or incomplete specification, oversized task scope, missing context or evidence, incorrect technical approach, dependency or environment problem, insufficient model capability, insufficient reasoning effort, architectural issue, or external blocker.

The next action follows the diagnosed cause.

## Retry vs Replanning

A retry repeats or corrects an execution within the existing technical approach. Replanning changes the approach, decomposition, dependencies, or Technical Plan. They are distinct operations.

When evidence shows that the current plan is incorrect or insufficient, the Lead Engineer stops retrying and moves the work to `REPLAN_REQUIRED`. Replanning may produce new or modified Atomic Tasks subject to applicable governance.

## Model Escalation

Model escalation is cause-based, not a fixed linear progression through every model or reasoning level. Insufficient reasoning may justify higher reasoning effort; insufficient capability may justify a model change; missing context requires context correction; specification problems require clarification or replanning; and external blockers require blocking rather than model escalation.

Specialists must not silently change their assigned model or reasoning configuration. The Lead Engineer controls classification and reclassification of Specialist executions. The Root Engineering Copilot controls classification and reclassification of Lead Engineer executions. Detailed mappings belong in `governance/model-routing.md`.

## Maximum Normal Model Escalation

The normal automatic routing ceiling is the highest execution configuration approved by the Model Routing Policy. If work remains unresolved after an appropriate GPT-5.6 Sol execution at the approved reasoning level, the system must not automatically increase reasoning, context size, retries, or compute indefinitely.

Instead:

STOP -> diagnose -> replan when possible -> escalate through the Copilot to the Human Operator when necessary

## External Blockers

When execution depends on something unavailable, the task becomes `BLOCKED`. Examples include an unavailable external service, missing authenticated capability, unavailable environment, unresolved dependency, required work in another project, missing product decision, or required Human decision.

Agents should not repeatedly spend tokens attempting to bypass a known external blocker.

## Completion Is a Termination Condition

Agents stop when the approved Atomic Task is complete. Completion means that all required conditions have been satisfied, including when applicable approved scope, acceptance criteria, deterministic verification, required Quality Engineering, required review, and required artifact updates.

Agents must not expand scope because related improvements are discovered. Unrelated improvements may be reported separately.

After technical completion:

TECHNICALLY_COMPLETE -> update operational work state -> STOP

The next Atomic Task requires separate authorization under the Task Lifecycle governance.

## Authority Boundaries

### Specialist

May make local implementation decisions required to complete the assigned Atomic Task within the approved specification and architecture. Decisions beyond that boundary must be escalated.

### Lead Engineer

May make technical decisions inside the approved Product Requirement, established architecture, approved security posture, and applicable execution-cost governance. The Lead Engineer owns technical diagnosis, replanning, and Specialist routing.

### Root Engineering Copilot

Owns workflow routing, Human Operator interaction, product-level coordination, classification and reclassification of Lead Engineer executions, and applicable Fast Path decisions. It must not replace the Lead Engineer as technical authority.

### Human Operator

Retains final authority over material changes to product intent, scope, architecture, security posture, significant execution cost, destructive or irreversible actions, and governance.

## Failure Flow

Conceptually:

Problem -> Specialist attempts bounded correction -> unresolved -> Lead Engineer diagnosis

If the Lead Engineer can resolve the issue within its authority, it may replan, reclassify, or reassign and continue through the normal lifecycle. If the issue exceeds Lead authority, it moves through the Root Engineering Copilot to the applicable Human Gate and Human decision.

## Initial Philosophy

Human Gates control consequential decisions; they do not require approval for normal engineering work. Termination Conditions prevent infinite retries, uncontrolled token consumption, blind model escalation, scope creep, and repeated work against external blockers.

The system remains autonomous inside approved boundaries and deliberately stops when those boundaries are reached.

