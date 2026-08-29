# GitHub MCP Strategy

This document defines the ownership, security boundaries, capability scope, and intended operational use of GitHub MCP in the initial Agentic Engineering system. It does not install or configure MCP.

## Core Purpose

GitHub is the intended operational source of truth for engineering work. GitHub MCP provides the authenticated capability to read and update that operational state. It is a capability layer, not repository memory or agent conversational memory.

## Initial Capability Owner

The Root Engineering Copilot is the primary owner of the GitHub operational capability:

Human -> Root Engineering Copilot <-> GitHub MCP -> Lead Engineer -> Specialists

Lead Engineer and Specialist agents do not receive GitHub MCP by default. They communicate through structured outcomes, artifacts, and the normal orchestration hierarchy. The Copilot translates approved engineering outcomes into operational GitHub state.

## Separation of Responsibilities

- Root Engineering Copilot: operational orchestration and GitHub interaction;
- Lead Engineer: technical authority and engineering coordination;
- Specialists: isolated execution of assigned engineering responsibilities;
- GitHub: operational work state;
- repository artifacts: durable technical knowledge.

Specialists are not required to access GitHub merely to determine current workflow state.

## Operational Flow

Before technical execution:

Copilot -> reads GitHub operational state -> identifies the current requirement and eligible Atomic Task -> assembles orchestration context -> routes work through the approved hierarchy

After technical execution:

Specialist -> Lead Engineer -> consolidated technical result -> Copilot -> GitHub operational state update -> STOP under Task Lifecycle governance

## Security

Use the official GitHub MCP Server. For local GitHub.com integration, prefer its OAuth flow and do not require a custom GitHub OAuth application when the official distribution provides an appropriate built-in flow. Do not introduce Personal Access Token authentication unless OAuth is proven unsuitable for an approved requirement.

Secure capability, never credentials. Authentication secrets and resulting access tokens must never enter prompts, repository files, `AGENTS.md`, committed configuration, environment documentation, agent artifacts, or model context. Use the pre-authenticated MCP capability.

## Least Capability

Do not expose every GitHub MCP tool by default. The approved initial toolsets are limited to:

- issues;
- projects.

The `context` toolset is optional and may be enabled only when a demonstrated approved workflow requires capability not provided by `issues,projects`. Do not enable it merely because it is part of a server default. Additional toolsets, including pull requests, actions, code security, and repository administration, require a later approved workflow. Use Least Capability + Least Context.

## GitHub Projects and Atomic Tasks

GitHub Issues / GitHub Projects are intended to represent operational work state. Future GitHub Workflow governance will define Project structure, fields, statuses, Product Requirement, Feature, Atomic Task, Issue and Pull Request relationships, and task ordering. Those details are not defined here.

Atomic Tasks derived from the approved Technical Specification and Implementation Plan should eventually become operational work items after the Consistency Gate passes:

Technical Specification -> Implementation Plan -> Atomic Tasks -> Consistency Gate PASS -> GitHub Issues / Project -> sequential execution

The exact Issue-generation mechanism is deferred.

## Read vs Write

During initial GitHub MCP validation, reads may execute normally while writes require explicit tool approval. This conservative restriction validates behavior safely.

After the workflow is proven, routine approved operational writes may later become automatically authorized according to governance. Potential writes include creating an approved Atomic Task Issue, updating operational status or a Project item, attaching implementation references, and recording a blocker. This document does not authorize unrestricted GitHub writes.

## High-Impact Operations

GitHub MCP availability does not bypass Human Gates. Force pushes, destructive repository operations, deletion of important remote resources, history rewriting, and consequential merge or release actions remain governed by applicable approval requirements. Capability does not imply authority.

## MCP Tool Context

Expose only tools needed for the current responsibility when practical. A smaller tool surface reduces unnecessary context, accidental tool selection, security exposure, and operational unpredictability. Broad toolsets must not be enabled merely for convenience.

## Failure Behavior

If GitHub MCP is unavailable:

- do not invent GitHub state;
- do not silently replace it with conversational memory;
- report the capability failure;
- preserve current durable artifacts;
- block operational updates that cannot be safely persisted.

Integration failure is not permission to create temporary repository progress files as a competing operational source of truth.

## Initial Validation Spike

GitHub MCP initial technical validation: PASS.

The validated scope is the official GitHub MCP Server over STDIO with OAuth, the `issues,projects` minimum toolset, Codex invocation, Issues reads and writes, Projects reads, write approval behavior, and the approved security boundary. Project write validation remains deferred until an appropriate Project item exists.

The validation spike used controlled test data where appropriate. It establishes technical MCP readiness; it does not implement the GitHub operational workflow.

## Implementation Status

This document defines governance only. GitHub MCP has passed initial technical validation, but the GitHub operational workflow remains future work. That work includes GitHub Project design, Project fields, Issue structure and templates, Product Requirement / Feature / Atomic Task mapping, workflow states, task publication rules, operational automation, and rules for routine automatic writes.

## Relationship With Existing Governance

GitHub MCP must operate consistently with the principles, orchestration, task lifecycle, agent contracts, context and artifact strategy, human gates, and tooling strategy defined in:

- `governance/principles.md`;
- `governance/orchestration.md`;
- `governance/task-lifecycle.md`;
- `governance/agent-contracts.md`;
- `governance/context-artifact-strategy.md`;
- `governance/human-gates.md`;
- `governance/tooling-strategy.md`.

Detailed policies remain owned by those documents.
