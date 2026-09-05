# Agent Input and Output Contracts

This document defines logical contracts and communication rules for the initial Agentic Engineering system. It does not define serialization formats, schemas, tooling, MCP, automation, token limits, or telemetry.

## Core Communication Principle

Agents communicate outcomes, evidence, artifacts, blockers, and decisions - not execution narratives.

Private reasoning and unnecessary execution history must not be transferred between agents.

## Minimum Sufficient Communication

Agent handoffs contain the minimum information required by the receiving agent to act correctly. Successful handoffs should be extremely terse. Unsuccessful handoffs contain only enough information to make the problem actionable.

Avoid long narrative summaries, repeated context, restating specifications already available as artifacts, explanations of obvious successful checks, step-by-step reasoning, and unnecessary implementation history. This reduces output tokens from the producing agent and input tokens consumed by the receiving agent.

Use Least Context and Just-in-Time Context principles. Agents should not decide their own verbosity when the caller can define the required output shape.

## Orchestration Metadata and Execution Context

Every Agent Run has logical orchestration metadata and an execution context. The physical format is intentionally undefined.

Orchestration metadata may include:

- execution ID;
- agent role;
- execution class;
- selected model;
- reasoning effort;
- runtime configuration.

This metadata belongs primarily to the orchestration and runtime layer. The Root Engineering Copilot selects the configuration for Lead Engineer executions. The Lead Engineer selects the configuration for Specialist executions. The executing agent runs under that configuration; it must not receive orchestration metadata merely because it exists.

The execution context contains only what the agent needs to perform the work. When applicable, it includes:

- task identifier;
- objective;
- scope and out of scope;
- acceptance criteria;
- relevant specification;
- repository instructions;
- relevant contracts;
- applicable constraints;
- artifact references;
- validation requirements;
- expected output.

Optional context is included only when required, such as dependencies, allowed tools or authenticated capabilities, or known blockers. Do not expose orchestration metadata to an agent as ordinary task content.

### Context Transfer

A caller transfers the receiving agent's current objective, acceptance criteria, scope, authority, target repository when applicable, resolved decisions, exact authoritative references, relevant evidence, and expected output. It transfers conclusions and references rather than raw exploratory output, repeated artifacts, private reasoning, or full conversation history.

Execution configuration remains in the orchestration layer. An agent executes under its assigned configuration and must not reread Model Routing merely to infer it. A receiving agent retrieves further detail only when needed; an independent evaluator verifies required claims from objective evidence.

## Expected Output Contract

Every delegated execution explicitly states the expected output type. Explicit output contracts are preferred over vague instructions such as “Be concise.”

If no blocking findings exist, the output may be `PASS`. If blocking findings exist, the output may be `CHANGES_REQUIRED` followed only by structured actionable findings.

## Common Outcome Categories

The general-purpose outcome vocabulary is:

- `COMPLETE`;
- `CHANGES_REQUIRED`;
- `BLOCKED`;
- `NEEDS_DECISION`;
- `NEEDS_RECLASSIFICATION`.

Specialized agents may additionally use `PASS`, `FAIL`, and `MANUAL_CHECK_REQUIRED` when appropriate. New statuses should not be introduced casually when an existing outcome is sufficient.

## Successful Output

Success is represented by the shortest response that preserves required information. A binary evaluation may be:

`PASS`

When evidence is required:

```text
COMPLETE

Artifacts:
- <references>

Validation:
PASS

Risks:
none
```

Narrative explanations are not required merely to confirm successful work.

## Actionable Failure Output

Failure remains concise but actionable and normally contains the finding or reason, evidence, and required action.

```text
CHANGES_REQUIRED

Finding:
Missing duplicate-email validation.

Evidence:
src/users/service.ts:84

Required action:
Handle the duplicate-email case according to the approved API contract.
```

Returning only `FAIL` is insufficient when the next agent needs information to correct the problem.

## Evidence and Artifact References

Prefer concrete evidence over explanatory prose. Evidence may reference files, lines, commits, pull requests, tests, builds, validation results, specifications, contracts, and safe relevant logs.

Large artifacts are referenced when they already exist in an accessible persistent location rather than copied into handoff messages. The handoff and the artifact are different things. The receiving agent should inspect a referenced artifact when more detail is required.

## No Private Reasoning Transfer

Agent contracts must never require private chain-of-thought or detailed internal reasoning. Do not request step-by-step reasoning, full thought processes, or everything an agent considered.

Agents return conclusions, decisions, evidence, artifacts, risks, blockers, and required actions. Context isolation applies to internal execution reasoning.

## Repository Engineer Contract

### Typical Input

When applicable: Atomic Task, relevant Technical Specification, acceptance criteria, repository memory, relevant contracts, validation requirements, and the context required for the assigned work.

For Repository Onboarding only, when the target repository has no local `AGENTS.md`, the input is the bounded onboarding objective, Lead Engineer context, applicable governance, direct repository evidence, and existing repository documentation/configuration. The Repository Engineer authors only the initial evidence-supported `AGENTS.md`; normal Product Work inputs and lifecycle resume after onboarding.

### Typical Successful Output

```text
COMPLETE

Artifacts:
- <references>

Validation:
PASS

Risks:
<only when relevant>
```

### Failure or Blocked Output

```text
BLOCKED

Reason:
<concise reason>

Evidence:
<reference when available>

Needs:
<what is required to continue>
```

The Repository Engineer must not provide unnecessary implementation narration.

## Quality Engineer Contract

### Typical Input

When applicable: Atomic Task, acceptance criteria, implementation artifacts, tests, deterministic validation evidence, and relevant Technical Specification.

### Successful Output

When no additional information is needed:

`PASS`

### Required Changes

```text
CHANGES_REQUIRED

Finding:
<concise finding>

Evidence:
<reference>

Required verification:
<what must be demonstrated>
```

### Manual Verification

When automated validation is not cost-effective:

```text
MANUAL_CHECK_REQUIRED

Checklist:
1. <action>
2. <action>
```

The checklist remains concise and reproducible. The Quality Engineer must not fix production implementation code.

## Code Reviewer Contract

### Typical Input

When applicable: Atomic Task, relevant Technical Specification, acceptance criteria, relevant diff, tests, deterministic validation evidence, and repository rules.

### Successful Output

`PASS`

No review summary is produced when there are no meaningful findings unless explicitly requested.

### Findings

```text
CHANGES_REQUIRED

Severity:
<severity>

Finding:
<concise finding>

Evidence:
<reference>

Required action:
<required change>
```

Multiple findings remain independently actionable and concise.

## Security Reviewer Contract

The Security Reviewer follows the same minimum-output philosophy. A successful evaluation is `PASS`.

When a security issue exists:

```text
CHANGES_REQUIRED

Severity:
<severity>

Category:
<security category>

Finding:
<concise description>

Evidence:
<reference>

Required action:
<required remediation>
```

General security commentary unrelated to the assigned scope is not included.

## Lead Engineer Contract

The Lead Engineer may produce larger durable artifacts because technical planning is itself an engineering deliverable. These may include a Technical Specification, ordered Atomic Task Plan, technical decisions, dependencies, and technical risks.

The handoff to the Root Engineering Copilot remains concise:

```text
PLANNING_COMPLETE

Spec:
<artifact reference>

Tasks:
6

Next:
BACK-001

Risk:
<only important risk, if any>
```

The complete Technical Specification is not copied into the Copilot handoff when it can be referenced as an artifact.

## Root Engineering Copilot Contract

The Copilot presents the Human Operator with consolidated outcomes rather than internal agent execution details. For example:

```text
BACK-001 complete.
Validation passed.
Next task: BACK-002.
Sol used: no.
Reference cost: <when available>
```

The Copilot may provide additional detail for a Human Gate, important risk, required decision, or explicit Human request. It must not forward every Specialist conversation to the Human Operator.

## Decision Contract

When Human or higher-level authority is required:

```text
NEEDS_DECISION

Decision:
<what must be decided>

Options:
A - <concise option>
B - <concise option>

Recommendation:
<recommended option when appropriate>

Impact:
<material impact only>
```

Questions must not be escalated without enough information for the decision maker to act.

## Reclassification Contract

When a Specialist determines that its current execution is insufficient:

```text
NEEDS_RECLASSIFICATION

Reason:
<concise diagnosed reason>

Relevant artifacts:
<references when applicable>
```

The Specialist does not need to know or report its exact execution class, model, or reasoning effort. It should not prescribe a stronger model unless there is a specific useful reason to recommend one. The Lead Engineer already owns the Specialist execution metadata and decides Specialist reclassification.

When the Lead Engineer determines that its current execution requires reassessment:

```text
NEEDS_RECLASSIFICATION

Reason:
<concise evidence that the current execution requires reassessment>

Relevant artifacts:
<references when applicable>
```

The Lead Engineer does not need to report exact execution metadata. It returns control to the Root Engineering Copilot, which owns Lead execution metadata and decides whether to increase reasoning, change model, correct context, replan, or take another appropriate action.

### Reclassification Context

Reclassification starts a new isolated Agent Run. The new run must not inherit the previous agent's private conversation or reasoning. Continuity comes from the same task, persistent artifacts, useful findings, evidence, concise structured handoff, and relevant current repository context.

Conceptually:

Agent Run A -> NEEDS_RECLASSIFICATION -> useful findings persist -> run ends

Routing authority reclassifies -> Agent Run B starts with new runtime configuration -> required task context and useful persisted results are delivered

## Blocked Contract

When work cannot continue:

```text
BLOCKED

Reason:
<concise blocker>

Evidence:
<reference when available>

Needs:
<dependency, capability, information, or decision required>
```

Agents must not repeatedly consume tokens describing or attempting to bypass a known blocker without new evidence.

## Minimal Success, Actionable Failure

Success should be extremely terse. Failure should be minimally actionable.

```text
PASS
```

```text
FAIL

Reason:
Expected HTTP 409 but received 500.

Evidence:
duplicate-email integration test.

Required:
Map the unique constraint violation according to the API contract.
```

## Output Size Philosophy

Use the minimum sufficient verbosity for the consumer. This document defines no hard numeric token limits, maximum output sizes, or quantitative efficiency thresholds. Those are governed by [governance/execution-budget.md](execution-budget.md); quantitative limits remain deferred pending telemetry-informed baselines.

Validated execution telemetry will inform whether stricter output limits improve cost efficiency without harming engineering quality.

## Relationship With Context Isolation

Input and output contracts are part of context isolation:

Caller -> minimum required input -> isolated Agent Run -> minimum required output -> Caller

Private execution context does not automatically propagate. Persistent artifacts and explicit structured handoffs are the approved collaboration mechanisms.

## Initial Communication Philosophy

The initial framework optimizes communication for low token overhead, high actionability, context isolation, traceability, deterministic parsing when possible, easy orchestration, and minimal duplication.

More text is not higher-quality communication unless the additional information is necessary for the next decision or execution.
