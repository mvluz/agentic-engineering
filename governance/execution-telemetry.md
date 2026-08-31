# Execution Telemetry and Token-Based Cost Accounting

This policy defines how the initial Agentic Engineering system measures token consumption and evaluates engineering efficiency. It does not represent the user's subscription cost or cash charged by OpenAI.

## Core Metric

The system must track token consumption and calculate an **API-Equivalent Reference Cost**: the hypothetical cost of measured usage if billed according to the applicable public OpenAI API token pricing.

This metric supports engineering cost analysis, model comparison, workflow optimization, agent efficiency analysis, project cost estimation, and cost-benefit learning over time. Subscription price, weekly Codex limits, included credits, and actual billing are outside this metric.

## Accounting Levels

### Workflow Execution

A Workflow Execution is the complete execution initiated for an approved unit of work. All Agent Runs belonging to it contribute to its total token consumption and reference cost. The Workflow Execution total is the primary accounting number.

Conceptually:

Copilot -> Lead Engineer -> Specialist -> Quality Engineer -> correction if required -> Code Reviewer -> Copilot

### Agent Run

An Agent Run is each isolated agent invocation inside a Workflow Execution. Retries, corrections, and repeated reviews are recorded as separate runs when the runtime makes this information available.

For example, one execution may contain a Copilot run, Lead Engineer run, Backend Engineer run, Quality Engineer run, Backend Engineer correction run, Code Reviewer run, and Copilot consolidation run.

### Aggregated Usage

Telemetry should eventually support aggregation by day, week, month, project, feature, task, agent, agent role, model, reasoning level, execution class, workflow, and project lifetime. The reporting implementation is intentionally undefined.

## Agent Run Data

When available, each Agent Run should record:

- execution ID and run ID;
- project, feature or requirement, and atomic task;
- agent or role;
- execution class, model, and reasoning effort;
- start timestamp, finish timestamp, and execution status;
- input tokens, cached input tokens, and output tokens;
- reasoning tokens when exposed by the runtime;
- API-Equivalent Reference Cost in USD;
- reference cost in BRL when calculated.

Telemetry contains operational usage data, not private chain-of-thought or full conversational context.

## Token Accounting

Token counts must come from measurable runtime usage information whenever available. Agents must not estimate or self-report their own consumption.

When total input, cached input, and output tokens are reported:

`uncached_input_tokens = input_tokens - cached_input_tokens`

Reference cost is calculated as:

`uncached input tokens * standard input price`

`+ cached input tokens * cached input price`

`+ output tokens * output price`

Reasoning tokens are preserved separately when available. Their billing treatment follows the pricing semantics applicable to the model and pricing snapshot.

## Reference Pricing

Reference cost calculations use a documented pricing snapshot containing the model, input price, cached input price, output price, pricing source, and pricing date or effective snapshot date.

Historical executions remain associated with the pricing snapshot used when their reference cost was calculated and must not silently change when pricing changes. The pricing storage mechanism is intentionally undefined.

## BRL Reference Cost

When calculated, BRL reference cost preserves the USD reference cost, USD/BRL exchange rate, exchange-rate source, exchange-rate date, and resulting BRL reference cost. BRL values are approximate economic references and must not be interpreted as actual charges.

## Telemetry Quality Levels

### FULL

Token usage is available for each relevant Agent Run. This is the preferred level.

### PARTIAL

Usage is available for some Agent Runs and/or additional execution totals are available, but complete per-agent attribution is not possible.

### EXECUTION_ONLY

Only the measured total for the complete Workflow Execution is available. This is acceptable when finer-grained runtime data is unavailable.

## No Fabricated Attribution

Per-agent usage must never be estimated or invented when the runtime does not provide sufficient data. Attribution priority is:

1. measured per-Agent-Run usage;
2. measured per-step usage;
3. measured Workflow Execution total.

A reliable execution total is more valuable than an artificial per-agent breakdown.

## Retry and Rework Accounting

Retries and correction loops are part of execution cost. When measurable, each additional run is preserved independently so analysis can identify cost introduced by implementation complexity, inadequate specifications, repeated review failures, insufficient model capability, excessive reasoning, unnecessary agent participation, or repeated corrections.

Rework must not be hidden inside a single success result.

## Cumulative Cost Analysis

The telemetry model should eventually support analysis of complete workflow, feature, project, role, task, review, and rework consumption. It should also support comparisons such as execution-class cost-to-quality ratios, model suitability, reasoning-effort value, and useful engineering output per million tokens.

The purpose is continuous optimization: do more useful engineering work with fewer tokens while preserving acceptable quality.

## Relationship With Model Routing

Execution Telemetry provides feedback to [governance/model-routing.md](model-routing.md):

Model Routing -> Execution -> Telemetry -> Cost and Quality Analysis -> Governance Improvement

Model-routing changes must be based on accumulated evidence rather than isolated impressions.

## Measurement Responsibility

Token accounting must be deterministic whenever possible:

Codex Runtime -> Usage Data -> Telemetry Collector -> Execution Ledger -> Reports

The agents must not spend reasoning effort calculating their own usage. The Copilot may read, summarize, and present telemetry, but must not invent token or cost values.

## Implementation Status

This document defines what must be measured and why. It does not define how telemetry will be collected or stored.

The current Codex runtime's relevant usage and relationship data has been validated through a technical spike, with version-sensitive limitations recorded in [docs/validation/execution-telemetry.md](../docs/validation/execution-telemetry.md). Storage technologies, file formats, collection scripts, and telemetry implementation remain intentionally undefined.
