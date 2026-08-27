# Model Routing Policy

This policy selects model capability and reasoning effort proportionally to task complexity and risk while keeping execution cost measurable and controlled.

## Core Principle

Model capability must be proportional to the complexity and risk of the work. The system should prefer the least expensive execution class that can reliably complete a task and must not use the strongest available model by default.

Model selection is explicit, traceable, and measurable. Agents must not freely select arbitrary models outside this policy.

Agent role != fixed model. Model selection is based on the complexity, risk, and nature of the current work, not only on organizational role. The same role may use different model and reasoning configurations for different work. For example, the Lead Engineer does not always run on GPT-5.6 Sol.

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

For normal engineering workflows, the Root Engineering Copilot controls classification and reclassification of Lead Engineer executions. The Lead Engineer controls classification and reclassification of Specialist executions. Each selects the execution class; this policy maps that class to the approved model and reasoning level.

Conceptually:

Human -> Copilot -> classifies Lead execution -> Lead Engineer -> classifies Specialist executions -> Specialist

An agent must not silently increase its own model or reasoning configuration. If its assigned configuration is insufficient, it reports the mismatch upward to the authority responsible for that execution.

### Lead Engineer Defaults

Normal technical planning uses GPT-5.6 Terra with Medium reasoning. Complex technical planning involving substantial tradeoffs, dependencies, or architectural reasoning uses GPT-5.6 Terra with High reasoning. Genuinely E4 Lead work uses GPT-5.6 Sol with High reasoning and emits the required non-blocking Sol notification.

GPT-5.6 Luna is not the normal baseline for technical planning. This initial guidance may later be optimized using execution telemetry.

## Sequential Task Classification

Execution classes apply to individual atomic executions, not necessarily to an entire feature. For example, one feature may contain an E3 technical design, E2 implementation tasks, an E1 mechanical configuration task, and an E3 independent critical review. Each execution is classified independently.

## Model and Reasoning Dimensions

Model capability and reasoning effort are independent routing dimensions. Routing is not a fixed ladder such as Luna / Low -> Luna / Medium -> Luna / High -> Terra / Low -> Terra / Medium -> Terra / High -> Sol / Low -> Sol / Medium -> Sol / High. A stronger reasoning level and a stronger model address different kinds of insufficiency.

## Cause-Based Escalation

Before increasing model capability or reasoning effort, diagnose why the current execution is insufficient.

- If the model is appropriate but deeper reasoning is needed, increase reasoning effort, such as Terra / Medium -> Terra / High.
- If greater model capability is required, change model without requiring every reasoning level first, such as Luna / Medium -> Terra / Medium.
- If context, evidence, dependencies, or repository information are missing, correct the context or block/replan rather than automatically increasing capability.
- If the specification, decomposition, or technical approach is invalid, stop or replan rather than repeatedly escalating models.

## Promotion

If an agent discovers that its assigned class or configuration is insufficient because the task is materially more complex or risky than expected, it must not silently switch to a stronger model. It should:

1. stop the current execution when appropriate;
2. report the complexity mismatch;
3. return control to the Lead Engineer or Copilot;
4. reclassify the task;
5. start a new execution using the higher approved class.

For example, an E2 Terra / Medium execution that discovers unexpected architectural complexity is stopped, escalated, reclassified as E3, and restarted with Terra / High. Promotion remains visible and traceable.

If the Lead Engineer determines that its own configuration is insufficient, it reports `MODEL_CLASS_MISMATCH` to the Root Engineering Copilot. The Copilot reclassifies the work and starts a new Lead execution. Specialists use the same principle by reporting to the Lead Engineer.

## Controlled Sol Promotion

E4 work initially uses GPT-5.6 Sol with High reasoning. When diagnosis shows that greater model capability is required but High reasoning is not also necessary, an authorized promotion may use GPT-5.6 Sol with Medium reasoning. If reasoning is later insufficient, it may be promoted to Sol / High.

This is a controlled routing decision, not a new execution class or automatic linear escalation ladder.

## Sol Usage Notification

Any execution using GPT-5.6 Sol must emit a non-blocking notification through the Root Engineering Copilot to the Human Operator, regardless of reasoning level, agent role, initial or promoted selection, or any future approved Sol reasoning configuration.

The notification is informational, does not require Human approval, and does not stop authorized execution. It should identify the task or execution, agent role, model, reasoning effort, reason Sol was selected, and whether selection was initial or promoted. The notification mechanism is not implemented by this policy.

Sol usage is `SOL_USAGE_NOTIFICATION`, not automatically a `HUMAN_GATE`. Human Gates remain governed separately by [governance/human-gates.md](human-gates.md).

## Downgrading

An execution must not be silently downgraded below its approved class. Future optimizations may change the default mapping only after sufficient telemetry demonstrates that a less expensive configuration provides acceptable quality.

## Cost Optimization

This policy is expected to evolve using measured execution telemetry, including token usage, reference cost, retries, rework, success or failure, and quality outcomes. Future analysis may ask whether E2 work can move from Terra / Medium to Luna / Medium, whether Terra / High improves results over Terra / Medium, when Sol / Medium is sufficient, which promotions reduce rework, and which routing decisions create unnecessary cost.

The long-term objective is the least expensive model and reasoning configuration that reliably provides the required engineering quality. Changes to the default mapping must be deliberate governance decisions rather than ad hoc agent choices.

Telemetry storage and token accounting are intentionally undefined here.

## Implementation Note

The framework intends to configure specialist executions with the model and reasoning level associated with their assigned class. The exact Codex implementation mechanism will be validated later through a dedicated technical spike. Model-routing behavior must not be considered operational until that validation is complete.
