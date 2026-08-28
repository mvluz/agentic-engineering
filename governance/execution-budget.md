# Execution Budget Policy

This policy controls token consumption and execution cost without optimizing for low usage at the expense of engineering quality.

The primary objective is:

`Maximize useful engineering outcome per token.`

## Core Principle

Execution cost is controlled deliberately. The goal is to achieve required engineering quality with the least model capability, context, reasoning, retries, and output necessary.

Optimize for useful engineering outcome per token, not minimum token usage at any cost.

## Workflow-Level Budget

Budget analysis considers the complete **Workflow Execution**, not only an individual Specialist run:

Copilot + Lead Engineer + Repository Engineer + Quality Engineer + correction runs + Code Reviewer + other required Specialists + Copilot consolidation = complete Workflow Execution cost

Individual Agent Runs remain measurable, but the complete Workflow Execution is the primary economic unit.

## No Arbitrary Numeric Limits Yet

The initial framework does not have enough telemetry to define trustworthy numeric token budgets. It does not establish per-task, per-review, or per-class token limits.

The initial strategy is:

MEASURE -> LEARN -> ESTABLISH BASELINES -> DEFINE QUANTITATIVE BUDGETS -> OPTIMIZE

Future numeric limits must be informed by actual execution telemetry.

## Execution Class Awareness

Expected resource consumption differs by execution class and workload. An administrative E0 execution is not evaluated using the same expectations as complex E3 or E4 engineering work.

Future baselines may consider execution class, agent role, task type, repository type, model, reasoning effort, and historical execution data. Numeric baselines are not defined here.

## Behavioral Guardrails

Before numeric budgets exist, token usage is controlled through approved behavior:

- one Atomic Task actively executed at a time;
- no parallel specialist execution;
- mandatory STOP after each Atomic Task;
- bounded correction loops;
- no unbounded model escalation;
- Sol usage notifications;
- Least Context and Just-in-Time Context;
- artifact-by-reference communication;
- minimum sufficient outputs;
- deterministic verification before reasoning-heavy evaluation.

These rules are part of the Execution Budget strategy.

## Soft Budget Warning

`SOFT_BUDGET_WARNING` indicates that an execution is consuming materially more resources than expected. It makes unusual consumption visible to the appropriate orchestration authority but does not automatically stop execution.

Quantitative thresholds will be defined later using telemetry.

## Hard Budget Limit

`BUDGET_LIMIT_REACHED` represents a future enforceable execution boundary. When an approved hard limit is reached:

STOP -> preserve useful execution state and artifacts -> diagnose excessive consumption -> determine whether to replan, reclassify, continue with Human approval, or terminate

Numeric limits and the enforcement mechanism are intentionally undefined.

## Context Budget

Input context is part of execution cost. Apply Least Context, Just-in-Time Context, targeted retrieval, and artifact references instead of unnecessary duplication.

Avoid loading complete conversation history, unrelated documentation, all future tasks, entire repositories when targeted retrieval is sufficient, or large artifacts merely because they may become useful. Use the smallest reliable context that enables correct execution.

Detailed context strategy is governed by [governance/context-artifact-strategy.md](context-artifact-strategy.md).

## Output Budget

Output tokens are part of execution cost. Use the Agent Contracts rule: “Use the minimum sufficient verbosity for the consumer.” Successful evaluations should normally be extremely terse, such as `PASS`. Failures should be minimally actionable, with a finding, evidence, and required action.

Do not define hard output-token limits in this policy. Future telemetry may justify quantitative budgets for specific Agent Run types.

## Retry Budget

Correction loops consume resources and remain bounded. For the same unresolved underlying problem, the initial rule allows at most two correction cycles before returning control to the Lead Engineer for diagnosis.

After the retry limit:

STOP automatic correction loop -> Lead Engineer diagnosis -> replan, reclassify, block, or escalate as appropriate

Replanning is not another automatic retry.

## Model Budget

Model capability is an economic resource. Use the Model Routing Policy to select the least expensive configuration expected to reliably perform the work. Do not use GPT-5.6 Sol merely as a default safety margin. Model and reasoning escalation remain cause-based rather than following a fixed ladder.

Any use of GPT-5.6 Sol generates the approved non-blocking Human notification. Detailed routing rules remain in [governance/model-routing.md](model-routing.md).

## Reasoning Budget

Higher reasoning effort is not automatically better. Use additional reasoning when the diagnosed workload requires it. Do not increase reasoning to compensate for missing context, invalid specifications, external blockers, unavailable evidence, or a flawed technical plan; correct those problems at their source.

Execution telemetry should determine when higher reasoning produces enough quality improvement to justify its additional cost.

## Rework Cost

Retries and rework remain visible as Workflow Execution cost. Correction runs must not be hidden inside a final successful result.

Telemetry should identify whether excessive cost originates from weak specifications, poor task decomposition, inadequate model selection, insufficient reasoning, implementation defects, repeated Quality Engineering or Code Review failures, unnecessary agent participation, or excessive context.

Reducing rework may provide greater savings than choosing a cheaper model.

## Quality Before False Economy

Token optimization must not materially reduce required engineering quality. False economy includes selecting a known-insufficient model, omitting required validation or independent review, removing necessary context, suppressing actionable failure evidence, or making tasks too large to avoid orchestration overhead.

Cost optimization remains subordinate to approved acceptance criteria, engineering quality, security, and governance.

## Telemetry-Driven Baselines

Future numeric budget policy must use measured telemetry. Potential measurements include tokens and API-Equivalent Reference Cost per Atomic Task and feature, cost by role, model, and execution class, retry and rework percentage, Quality Engineering and Code Review cost, Sol usage frequency, and success rate by routing configuration.

The purpose is to establish realistic baselines from actual Agentic Engineering workflows.

## Cost-to-Quality Optimization

Telemetry should support experiments comparing Luna / Medium with Terra / Medium, Terra / Medium with Terra / High, Sol promotions, more expensive planning against downstream implementation cost, and unnecessary output or context consumption.

Budget and routing policy should evolve from measured evidence rather than intuition alone.

## Relationship With Other Governance

Execution Budget works with:

- [governance/model-routing.md](model-routing.md) for model and reasoning selection;
- [governance/execution-telemetry.md](execution-telemetry.md) for measured usage and API-Equivalent Reference Cost;
- [governance/agent-contracts.md](agent-contracts.md) for minimum sufficient communication;
- [governance/context-artifact-strategy.md](context-artifact-strategy.md) for context efficiency;
- [governance/human-gates.md](human-gates.md) for bounded correction and escalation;
- [governance/task-lifecycle.md](task-lifecycle.md) for sequential Atomic Task execution and mandatory stopping boundaries.

Detailed policies remain owned by those documents.

## Initial Policy Summary

1. Budget the complete Workflow Execution, not only individual agents.
2. Do not invent numeric limits before sufficient telemetry exists.
3. Use behavioral guardrails immediately.
4. Distinguish Soft Budget Warnings from Hard Budget Limits.
5. Control input context and output verbosity.
6. Bound retries and rework.
7. Use the least expensive model configuration that reliably meets quality requirements.
8. Diagnose failures before increasing model or reasoning capability.
9. Use telemetry to establish future quantitative thresholds.
10. Optimize useful engineering outcome per token.
11. Never sacrifice required engineering quality merely to reduce token consumption.

## Deferred Decisions

This policy does not define numeric token, USD, or BRL thresholds; per-agent or per-class caps; automatic enforcement; runtime kill mechanisms; warning or hard-limit thresholds; output or context maximums; or cost-forecasting algorithms.

