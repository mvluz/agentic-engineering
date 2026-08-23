# Agentic Engineering Principles

Initial version.

These principles govern the agents, workflows, and projects that adopt the Agentic Engineering framework. They define the baseline expectations for authority, specification, collaboration, security, verification, autonomy, and traceability.

## 1. Human Authority

The human operator is the final authority. Agents may operate autonomously within defined boundaries, but critical decisions must include human approval gates.

## 2. Spec Before Implementation

Relevant work must not begin until there is a clear specification containing the objective, scope, constraints, and acceptance criteria.

## 3. Atomic Work

Every execution should be small, bounded, verifiable, and appropriate for the available context and token budget.

## 4. Context Isolation

Each agent operates within its own context. No agent automatically inherits the private execution context of another agent.

## 5. Artifact-Based Collaboration

Agents collaborate through persistent artifacts such as specifications, code, commits, pull requests, tests, contracts, and reports rather than conversational history.

## 6. Externalized State

Project state must exist outside LLM memory, in artifacts such as GitHub, documentation, issues, specifications, code, tests, and CI systems.

## 7. Least Context / Just-in-Time Context

Each agent should receive only the context required to perform its task and retrieve additional information only when necessary.

## 8. Bounded Autonomy

Autonomous loops are allowed, but they must have retry limits, stopping conditions, and escalation mechanisms.

## 9. Deterministic Verification First

Builds, linting, tests, schema validation, and other deterministic checks should validate everything that can be objectively verified before relying on LLM-based judgment.

## 10. Independent Evaluation

When independent evaluation is required, the evaluating agent must not share the private execution context of the agent that produced the work.

## 11. Secure Capability, Never Credentials

Agents use authenticated capabilities already configured in the environment. Credentials and secrets must never enter prompts, agent context, documentation, logs, generated artifacts, or Git.

## 12. Cost-Aware Engineering

Workflow complexity must be proportional to task complexity and risk. Do not use multiple agents or more expensive execution modes when a simpler execution is sufficient.

## 13. Traceability

Relevant changes should be traceable from intent to delivery, ideally following a chain such as:

Requirement -> Spec -> Task -> Issue -> Implementation -> Tests/Review -> PR -> Deployment.
