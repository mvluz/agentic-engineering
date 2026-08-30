# GitHub Operational Workflow

This document defines the initial GitHub Workflow for Agentic Engineering. It governs operational work state and does not implement tooling or configure GitHub.

## Operational Hierarchy

Use native GitHub parent/sub-issue relationships:

`Epic -> Feature -> User Story -> Task`

These are distinct hierarchy levels. The topology of Projects and repositories remains intentionally deferred: this document does not require one Project per repository, one shared Project, or one Epic per repository.

## Project Fields

The minimum Project fields are:

`Work Type`: `Epic`, `Feature`, `User Story`, `Task`

`Priority`: `P0`, `P1`, `P2`, `P3`

Human-facing `Status` values map to the canonical governance states:

| Human-facing value | Canonical state |
| --- | --- |
| Planned | `PLANNED` |
| Ready | `READY` |
| In Progress | `IN_PROGRESS` |
| Blocked | `BLOCKED` |
| Need Decision | `NEEDS_DECISION` |
| Validating | `VALIDATING` |
| Changes Required | `CHANGES_REQUIRED` |
| Technically Complete | `TECHNICALLY_COMPLETE` |
| Done | `DONE` |

`TECHNICALLY_COMPLETE != DONE`.

## Execution

Execution is atomic and sequential: Tasks are always executed one at a time.

Human authorization and validation may cover one Task, User Story, or Feature. When a larger scope is authorized, its Tasks still execute sequentially. The Copilot does not need a new authorization between every Task unless a mandatory Human Gate or blocker occurs. The Human chooses the delivery and validation boundary.

Priority does not define execution order by itself. Order comes from the approved Technical Plan, task sequencing, dependencies, and current Human authorization.

## Dependencies

Use native GitHub `blocked by` / `blocking` relationships only for real technical dependencies. Do not create artificial dependencies merely to enforce sequential execution; sequencing is an orchestration rule.

## Source of Truth

GitHub owns operational work state, including hierarchy, status, blockers, current and next eligible work, and applicable Issue, Pull Request, commit, and evidence references.

Repository documentation owns durable technical knowledge, including Technical Specifications, Implementation Plans, architecture, contracts, durable decisions, and technical documentation. Do not duplicate large technical documents inside Issues.

Operational work is materialized only after:

`Product Requirement -> Technical Specification -> Implementation Plan -> Atomic Tasks -> Consistency Gate PASS -> GitHub materialization`

## GitHub Ownership

The Root Engineering Copilot is the primary operational owner of GitHub MCP. Lead and Specialist agents do not receive GitHub MCP by default. They return outcomes, evidence, blockers, and artifact references to the Copilot, which persists operational state.

## MCP Capability and Manual Setup

The validated official GitHub MCP can create Projects and Issues, add Issues to Projects, assign values to existing Project fields, update Status, create and read native parent/sub-issue relationships, and reconstruct configured operational state.

The tested MCP cannot create arbitrary Project custom fields or configure their single-select options. One-time Human/manual setup is therefore acceptable for `Work Type`, `Priority`, and lifecycle `Status` options. After those fields exist, normal operation can be MCP-driven. Manual setup is not an architectural blocker.

## Free-Tier Invariant

The GitHub Workflow must not require a paid GitHub plan, a trial-only feature, or a metered capability that creates required operational charges. If a required capability becomes unsuitable for GitHub Free, replace or remove that dependency rather than requiring a paid upgrade.

GitHub Actions is not required for this operational workflow. CI/CD remains a separate concern.

## Cost and Telemetry

Do not add model, reasoning, token, or reference-cost telemetry as Project fields by default. These belong to runtime orchestration and telemetry unless future evidence-based governance changes that decision.

## Relationship With Existing Governance

This workflow remains consistent with:

- `governance/task-lifecycle.md`;
- `governance/orchestration.md`;
- `governance/agent-contracts.md`;
- `governance/context-artifact-strategy.md`;
- `governance/execution-state.md`;
- `governance/human-gates.md`;
- `governance/github-mcp.md`.

Detailed policies remain owned by those documents.
