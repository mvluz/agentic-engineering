# Model Routing Policy

This policy selects model capability and reasoning effort proportionally to task complexity and risk while keeping execution cost measurable and controlled.

## Core Principle

Model capability must be proportional to the complexity and risk of the work. The system should prefer the least expensive execution class that can reliably complete a task and must not use the strongest available model by default.

Model selection is explicit, traceable, and measurable. Agents must not freely select arbitrary models outside this policy.

## Execution Classes

### E0 - Administrative / Mechanical Work

For repository administration, already-approved documentation, small file organization, mechanical edits, simple metadata changes, and highly deterministic tasks with almost no ambiguity.

Default: GPT-5.6 Luna, Low reasoning.

Example: creating an approved governance Markdown document from an explicit specification.

### E1 - Simple, Well-Specified Work

For small tasks with clear acceptance criteria, low-risk repository changes, straightforward implementation work, and tasks requiring some interpretation but little technical uncertainty.

Default: GPT-5.6 Luna, Medium reasoning.

### E2 - Normal Engineering Work

For normal feature implementation, backend or frontend engineering from an approved Technical Specification, bounded repository-level refactoring, integration work with known contracts, and engineering tasks requiring meaningful reasoning without exceptional architectural risk.

Default: GPT-5.6 Terra, Medium reasoning.

### E3 - Complex Engineering / Critical Review

For technically complex implementation, difficult debugging, important independent code review, complex cross-component reasoning, high-risk migrations, and tasks with substantial ambiguity or technical tradeoffs.

Default: GPT-5.6 Terra, High reasoning.

### E4 - Architecture / Security / Frontier Difficulty

Only for genuinely high-complexity or high-consequence work, such as major architectural decisions, complex cross-project design, difficult security analysis, fundamental technology decisions, severe or unusual production problems, or problems where lower classes have proven insufficient.

Default: GPT-5.6 Sol, High reasoning.

## Initial Mapping

| Class | Workload | Model | Reasoning |
|------|----------|-------|-----------|
| E0 | Administrative / mechanical | GPT-5.6 Luna | Low |
| E1 | Simple and well-specified | GPT-5.6 Luna | Medium |
| E2 | Normal engineering | GPT-5.6 Terra | Medium |
| E3 | Complex engineering / critical review | GPT-5.6 Terra | High |
| E4 | Architecture / security / frontier difficulty | GPT-5.6 Sol | High |

## Reasoning Limits

XHigh, Max, or equivalent highest reasoning modes are not used by default in the initial framework. Higher reasoning levels may be evaluated later only when operational evidence shows that the quality benefit justifies the cost.

## Classification Responsibility

For Fast Path administrative or trivial work, the Root Engineering Copilot may classify work as E0 or E1.

For normal engineering workflows, the Lead Engineer classifies each atomic engineering task before delegating it. The Lead Engineer selects the execution class; this policy maps that class to the approved model and reasoning level.

## Sequential Task Classification

Execution classes apply to individual atomic executions, not necessarily to an entire feature. For example, one feature may contain an E3 technical design, E2 implementation tasks, an E1 mechanical configuration task, and an E3 independent critical review. Each execution is classified independently.

## Promotion

If an agent discovers that its assigned class is insufficient because the task is materially more complex or risky than expected, it must not silently switch to a stronger model. It should:

1. stop the current execution when appropriate;
2. report the complexity mismatch;
3. return control to the Lead Engineer or Copilot;
4. reclassify the task;
5. start a new execution using the higher approved class.

For example, an E2 Terra / Medium execution that discovers unexpected architectural complexity is stopped, escalated, reclassified as E3, and restarted with Terra / High. Promotion remains visible and traceable.

## Downgrading

An execution must not be silently downgraded below its approved class. Future optimizations may change the default mapping only after sufficient telemetry demonstrates that a less expensive configuration provides acceptable quality.

## Cost Optimization

This policy is expected to evolve using execution telemetry. Future analysis may compare model and reasoning configurations, measure role and class costs, and evaluate cost-to-quality ratios. Changes to the default mapping must be deliberate governance decisions rather than ad hoc agent choices.

Telemetry storage and token accounting are intentionally undefined here.

## Implementation Note

The framework intends to configure specialist executions with the model and reasoning level associated with their assigned class. The exact Codex implementation mechanism will be validated later through a dedicated technical spike. Model-routing behavior must not be considered operational until that validation is complete.

