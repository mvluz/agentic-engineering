# Agent Input and Output Contracts

This document defines logical contracts and communication rules for the initial Agentic Engineering system. It does not define serialization formats, schemas, tooling, MCP, automation, token limits, or telemetry.

## Core Communication Principle

Agents communicate outcomes, evidence, artifacts, blockers, and decisions - not execution narratives.

Private reasoning and unnecessary execution history must not be transferred between agents.

## Minimum Sufficient Communication

Agent handoffs contain the minimum information required by the receiving agent to act correctly. Successful handoffs should be extremely terse. Unsuccessful handoffs contain only enough information to make the problem actionable.

Avoid long narrative summaries, repeated context, restating specifications already available as artifacts, explanations of obvious successful checks, step-by-step reasoning, and unnecessary implementation history. This reduces output tokens from the producing agent and input tokens consumed by the receiving agent.

Use Least Context and Just-in-Time Context principles. Agents should not decide their own verbosity when the caller can define the required output shape.

## Common Input Contract

Every Agent Run receives a logical input envelope. The physical format is intentionally undefined.

When applicable, input contains:

- execution identifier;
- task identifier;
- agent role;
- objective;
- scope and out of scope;
- acceptance criteria;
- relevant technical context;
- applicable constraints;
- relevant artifact references;
- expected output;
- execution class;
- assigned model and reasoning configuration.

Optional information is included only when required, such as dependencies, technical contracts, validation requirements, allowed tools or authenticated capabilities, known blockers, and repository-specific instructions.

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

When applicable: Atomic Task, relevant Technical Specification, acceptance criteria, repository memory, relevant contracts, validation requirements, and execution configuration.

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

When a Specialist determines that its assigned configuration is insufficient:

```text
NEEDS_RECLASSIFICATION

Assigned:
<execution class, model, reasoning>

Reason:
<concise diagnosed reason>

Suggested:
<recommended class or configuration when appropriate>
```

The Specialist must not change its own configuration. The Lead Engineer decides Specialist reclassification.

When the Lead Engineer determines that its own configuration is insufficient, it uses the same structure with `Current` instead of `Assigned`, then returns control to the Root Engineering Copilot. The Copilot decides Lead Engineer reclassification.

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

Use the minimum sufficient verbosity for the consumer. This document defines no hard numeric token limits, maximum output sizes, or quantitative efficiency thresholds. Those belong to a future Execution Budget Policy.

Execution telemetry should later determine whether stricter output limits improve cost efficiency without harming engineering quality.

## Relationship With Context Isolation

Input and output contracts are part of context isolation:

Caller -> minimum required input -> isolated Agent Run -> minimum required output -> Caller

Private execution context does not automatically propagate. Persistent artifacts and explicit structured handoffs are the approved collaboration mechanisms.

## Initial Communication Philosophy

The initial framework optimizes communication for low token overhead, high actionability, context isolation, traceability, deterministic parsing when possible, easy orchestration, and minimal duplication.

More text is not higher-quality communication unless the additional information is necessary for the next decision or execution.

