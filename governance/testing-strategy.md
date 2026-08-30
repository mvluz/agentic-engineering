# Testing and Quality Strategy

This document defines the initial testing responsibilities, deterministic verification, repository testing memory, optional Quality Engineering, and relationship between Feature completion and deployment.

## Core Principle

Repository Engineers validate Atomic Tasks. The Human decides whether additional Feature-level Quality Engineering is required.

Testing responsibility is primarily part of engineering implementation. Quality Engineering is an optional capability in v1, not a mandatory stage for every Atomic Task or Feature.

## Repository Engineer Responsibility

The Repository Engineer responsible for an Atomic Task owns the technical tests and deterministic verification required by that task. Depending on the repository and risk, this may include unit, integration, contract, component, schema, migration, build, lint, formatting, type-checking, or other repository-specific checks.

Implementation and relevant tests should normally be delivered together. Testing must not be deferred on the assumption that a Quality Engineer will automatically follow.

## Deterministic Verification First

Before an Atomic Task reaches technical completion, all applicable deterministic checks defined by the repository or Technical Specification must pass:

Atomic Task -> implementation -> relevant tests -> deterministic verification -> technical completion

If verification fails, return to the Repository Engineer, correct the implementation or tests, and verify again. Do not invoke reasoning-heavy Quality Engineering merely to discover failures already exposed by deterministic tooling.

Tests should derive from the Technical Specification, acceptance criteria, technical contracts, identified risk, and expected behavior. Relevant acceptance conditions require suitable evidence through automated tests, deterministic verification, Human validation, or on-demand Quality Engineering as appropriate to the work and its risk.

## Practical Testing Strategy

Use the cheapest reliable test that proves the required behavior. Do not impose universal unit, integration, or end-to-end ratios.

- unit tests: isolated behavior;
- integration tests: interaction between real components;
- contract tests: system or API boundaries;
- end-to-end tests: important complete workflows.

Backend and API repositories should favor repeatable automated verification where practical, including API, contract, database, and migration checks when relevant. Do not test against production environments or databases.

Frontend and mobile repositories should use appropriate deterministic checks such as unit or component tests, lint, type checking, build validation, and repository-specific automated tests. Visual navigation or full UI automation is not mandatory merely because tooling exists. Add browser or device automation when risk, repeated validation effort, regression frequency, critical user flows, expected reuse, and cost justify it.

## Quality Engineer Is On-Demand

The Quality Engineer is not automatically invoked after every Atomic Task or Feature and is not a mandatory delivery gate in v1. The Human Operator decides whether to request Quality Engineer involvement.

When explicitly requested, the Quality Engineer may evaluate Feature quality, review coverage and evidence, identify scenarios or edge cases, advise the Human, create or execute additional tests, evaluate regressions, or provide a broader quality assessment. The scope must follow the Human's intent.

The Quality Engineer must not silently take ownership of production implementation fixes. Production defects return through the approved engineering workflow to the responsible Repository Engineer.

## Human Validation Authority

After Feature implementation is technically ready, the Human Operator decides the desired level of additional validation. The Human may approve directly, test personally, request Quality Engineer guidance, request Quality Engineer evaluation, or request additional automated or manual testing.

Do not automatically create a Human testing checklist or require a predefined testing ceremony. A checklist is produced only when the Human requests one, a Quality Engineer is explicitly asked to provide one, or another approved workflow requires one.

## Repository Testing Memory

Every executable repository should maintain sufficient durable documentation describing how it can be tested when applicable, using `docs/testing/` or another appropriate repository-specific structure. This memory may include project startup, test/build/lint/type-check commands, environment prerequisites, test databases, fixtures, mocks, test authentication, browser or device requirements, integration setup, and stable manual validation procedures.

`AGENTS.md` should contain only essential testing instructions and references. It is the operating map; detailed testing procedures belong in authoritative testing documentation.

Testing documentation describes how to test, not transient execution evidence. Do not use it for current Feature status, blockers, task completion, or temporary results. Operational state belongs in GitHub Issues / Projects. Update testing documentation when implementation changes durable testing knowledge; do not modify it mechanically when nothing durable changed.

## Test Quality, Regression, and Cost

Optimize for test quality and risk coverage, not test count or coverage percentage alone. Tests should protect required behavior, important contracts, meaningful edge cases, high-risk logic, and likely regressions.

When a real defect is discovered, prefer a suitable regression test when technically practical:

reproduce defect -> add regression evidence -> fix implementation -> verify

Automation must remain cost-aware. Automate when repeatability, risk, or expected reuse justifies implementation and execution cost, while avoiding both low-value automation and repeated expensive manual validation of critical workflows.

## Atomic Task Completion

An Atomic Task may reach technical completion when its required implementation and deterministic validation obligations succeed. Quality Engineer participation is not required unless explicitly requested or required by another approved policy.

Repository Engineer -> implementation -> relevant tests -> deterministic verification -> `TECHNICALLY_COMPLETE` -> operational state persistence -> `DONE` -> STOP

The existing Task Lifecycle and Execution State rules remain applicable.

## Feature-Level Flow

When all planned Atomic Tasks for a Feature are complete:

Atomic Tasks complete -> Convergence Check -> Feature ready -> Human evaluation and optional testing -> Human approval -> deployment -> finalization

The Convergence Check determines whether implementation satisfies the approved upstream engineering artifacts as a whole. Human validation represents the Human's chosen final evaluation. Quality Engineering provides optional additional expertise. These responsibilities must not be collapsed into a mandatory QA stage.

The overall Feature is not finally complete merely because implementation tasks are technically complete. Human approval occurs before final deployment completion. Deployment mechanics and post-deployment verification are governed separately and are not implemented here.

## Validation Failure

If Human validation or on-demand Quality Engineering identifies a defect, record the actionable finding, return it to the Lead Engineer, create or update the required Atomic Task(s), and resume the normal sequential workflow after Repository Engineer correction and deterministic verification. Do not apply untracked production fixes outside the approved lifecycle.

## Relationship With Existing Governance

Testing Strategy remains consistent with:

- `governance/task-lifecycle.md`;
- `governance/agent-contracts.md`;
- `governance/repository-agent-strategy.md`;
- `governance/context-artifact-strategy.md`;
- `governance/execution-budget.md`;
- `governance/execution-state.md`;
- `governance/human-gates.md`.

Detailed policies remain owned by those documents.

## Initial Strategy Summary

1. Repository Engineers own implementation-related tests.
2. Deterministic verification precedes technical completion.
3. Tests derive from specifications, acceptance criteria, contracts, and risk.
4. Use the cheapest reliable test that proves required behavior.
5. Do not impose rigid universal testing ratios.
6. Quality Engineering is fully on-demand in v1.
7. The Human decides whether and how Quality Engineering participates.
8. No automatic Human testing checklist is generated.
9. The Human may approve directly, test personally, or request Quality Engineering.
10. Executable repositories maintain durable testing documentation when applicable.
11. `AGENTS.md` references detailed testing memory rather than duplicating it.
12. Testing documentation describes how to test, not current test status.
13. Test quality matters more than test count.
14. Prefer regression tests for real defects when practical.
15. Testing automation remains cost-aware.
16. Feature-level Human approval follows implementation and Convergence Check and precedes final deployment completion.

## Deferred Decisions

Do not define yet:

- mandatory testing frameworks;
- coverage percentage requirements;
- mandatory end-to-end tools;
- Playwright or Appium requirements;
- CI pipeline implementation;
- deployment workflow;
- post-deployment smoke-test implementation;
- Quality Engineer agent configuration;
- numeric testing token budgets;
- automatic Quality Engineer invocation rules.

These decisions belong to later project-specific or implementation stages.
