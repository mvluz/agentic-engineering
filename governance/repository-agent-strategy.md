# Repository Agent Strategy

This document defines how repository-specific context, engineering specialization, and durable repository memory are organized in the initial Agentic Engineering system.

## Core Principle

A Repository Engineer is specialized by the repository context in which it executes, not by carrying permanent knowledge of every project.

Repository-specific knowledge remains close to the repository rather than accumulating inside global agents.

## Initial Repository Convention

The default conceptual structure is:

```text
repo/
├── AGENTS.md
├── .codex/
├── src/
├── docs/
│   ├── architecture/
│   ├── development/
│   ├── testing/
│   ├── api/
│   ├── decisions/
│   └── works/
│       └── usXXXX/
│           ├── spec.md
│           ├── plan.md
│           └── other durable work artifacts when required
└── other project-specific files
```

This is a default architectural convention, not a requirement to create every directory. Directories should exist only when needed, and technology-specific structure may override the convention when justified.

`src/` is the preferred default location for primary source code, not an absolute cross-technology requirement.

## AGENTS.md

`AGENTS.md` contains the essential, stable instructions required by agents working in the repository. It should be concise, versioned, durable, immediately relevant, and repository-specific. It acts as the repository's essential operating map, not its technical encyclopedia.

Typical content includes the repository purpose, a concise architecture summary, essential development and validation commands, critical rules, project constraints, repository-specific security rules, and references to authoritative detailed documentation. It should point to detailed documentation rather than duplicate it.

`AGENTS.md` must not contain current task status, next-task or roadmap state, GitHub Project state, execution telemetry, token usage, execution cost, conversational history, credentials, secrets, machine-specific configuration, large duplicated architecture documentation, or global Agentic Engineering governance.

## Repository Onboarding

Repository Onboarding is a narrow bootstrap exception before normal Product Work. An explicit Human request authorizes the Root Engineering Copilot to coordinate onboarding of one existing repository so it can receive its first repository-local `AGENTS.md`.

The flow is:

Human requests onboarding -> Root Engineering Copilot coordinates -> Lead Engineer analyzes repository context -> Repository Engineer creates initial `AGENTS.md` -> Lead validates and consolidates -> Root Copilot reports to Human

This bootstrap does not require a Product Requirement, Technical Specification, Implementation Plan, Atomic Task, Consistency Gate, GitHub Issue, GitHub Project item, or work hierarchy. Normal Product Work begins only after onboarding when a Product Requirement is requested.

The default permitted mutation is `<target-repository>/AGENTS.md`. The assigned Repository Engineer may inspect the target repository read-only, including its Git state, structure, source, configuration, tests, CI, documentation, architecture evidence, and local conventions. The resulting `AGENTS.md` contains only durable repository-specific operating knowledge supported by that evidence.

Onboarding must not modify application code, dependencies, CI, infrastructure, tests, or existing documentation unless the Human explicitly authorizes expanded scope. If inspection identifies a material product, architecture, security, infrastructure, dependency, destructive, or significant process change, stop and escalate it to the Root Copilot for normal Product Lifecycle handling.

The Repository Engineer resolves and confirms the target repository root, searches and mutates only within it, and never modifies the Agentic Engineering framework repository while onboarding another repository. The Root may coordinate across repositories, but each child execution remains repository-bounded.

GitHub Issues / Projects remain the operational source of truth for normal Product Work after materialization. Repository Onboarding does not require GitHub access or fake operational work; any GitHub operation requires an explicit Human request and the approved GitHub MCP capability.

## .codex/

`.codex/` is reserved for repository-specific Codex runtime configuration or resources when required. Potential future uses include repository-specific Codex configuration, local agent definitions, runtime behavior, Skills configuration, justified MCP configuration, and supported sandbox or approval behavior.

General technical documentation belongs under `docs/`, not `.codex/`.

Repository-specific `.codex/` content may be versioned when it is safe to share, required for consistent project execution, repository-specific, and free of secrets or personal machine configuration. Credentials, OAuth tokens, personal authentication settings, machine-specific secret paths, and private user settings must remain outside the repository.

## Repository Documentation

`docs/` contains durable technical repository memory. It may include architecture, development, testing, API, decisions, and work-specific documentation. Empty directories are not required.

Use `docs/works/<work-id>/` for durable technical artifacts associated with a Product Requirement, User Story, or approved work item. For example:

```text
docs/works/us1234/
├── spec.md
├── plan.md
└── other durable artifacts when required
```

For the initial convention, `spec.md` is the Technical Specification and `plan.md` is the Implementation Plan. Additional contracts, decisions, schemas, migration designs, or other artifacts may exist when technically necessary. Do not create files merely for completeness.

Work folders preserve durable engineering knowledge; they do not track current execution progress. Do not use them for task checkboxes, current or next task, operational status, active blockers, execution progress, or GitHub workflow state. GitHub Issues / GitHub Projects remain the operational source of truth.

## Planning and Operational State

For normal Product Work, the approved derivation model remains:

Product Requirement -> Technical Specification -> Implementation Plan -> Atomic Tasks

The work folder preserves the durable Specification and Plan. After the required Consistency Gate, Atomic Tasks become operational work in GitHub. Repository Markdown task lists must not become the authoritative operational tracker.

The distinction is:

- repository: durable engineering knowledge and artifacts;
- GitHub Issues / Projects: operational work state.

## Repository Engineer Creation

A Repository Engineer is an isolated Specialist execution created by the Lead Engineer for work in one repository. For normal Product Work:

Lead Engineer -> identifies the responsible repository -> launches Repository Engineer -> repository instructions apply -> relevant context is retrieved -> Atomic Task is executed

The Repository Engineer becomes specialized through `AGENTS.md`, applicable `.codex/` resources, relevant documentation, `docs/works/<work-id>/` artifacts, current source code, and the Atomic Task Execution Context Pack. During Repository Onboarding only, when no local `AGENTS.md` exists, it instead uses the bounded Lead context, applicable governance, direct repository evidence, and existing documentation/configuration to author the first one. A global implementation agent must not permanently carry detailed knowledge of every repository.

## Technology Specialization

The organizational role remains Repository Engineer while repository and task context determine practical specialization. A Repository Engineer in an API repository may operate as a Backend Engineer; in a mobile repository as a Mobile Engineer; in a frontend repository as a Frontend Engineer; and in an infrastructure repository according to the relevant Platform responsibility.

## One Repository Per Execution

In the initial version, a Repository Engineer executes against one repository at a time. A Feature spanning multiple systems must not give one implementation Specialist simultaneous responsibility for multiple repositories.

For multi-repository work, the Lead Engineer decomposes the Implementation Plan into repository-bounded Atomic Tasks, routes each task to the appropriate Repository Engineer, and executes them sequentially according to dependencies. This preserves context isolation, clear ownership, smaller context, lower token consumption, and repository-specific validation.

## Cross-Repository Contracts

Repositories that depend on one another communicate through durable authoritative contracts rather than shared private execution context. Examples include API contracts, schemas, interface definitions, migration contracts, and other approved cross-repository artifacts.

For example, a Backend Repository Engineer may produce or update an authoritative API contract, and a Mobile Repository Engineer may receive that relevant contract. The Mobile Engineer does not receive the Backend Engineer's private conversation or complete repository context.

## One Authoritative Home

Reference the authoritative source instead of copying the same knowledge into multiple artifacts. The conceptual ownership is:

- Agentic Engineering governance: global framework rules;
- `AGENTS.md`: essential repository operating rules and navigation;
- `.codex/`: Codex-specific runtime and configuration resources;
- `docs/`: detailed durable repository knowledge;
- `docs/works/<work-id>/`: durable work-specific technical artifacts;
- Skills: reusable procedures or capabilities;
- GitHub Issues / Projects: operational work state.

Durable facts should have one authoritative home whenever practical. An `AGENTS.md` may reference a migration policy rather than duplicate it, and an Atomic Task may reference an API contract rather than copy it.

## Repository Memory Maintenance

Updating durable repository memory is part of an Atomic Task when implementation changes a durable technical truth needed to understand, develop, test, operate, or maintain the repository. This includes architecture, development commands, testing procedures, API or integration contracts, migration conventions, repository coding rules, and important technical decisions.

If implementation knowingly makes an authoritative artifact incorrect, the responsible Repository Engineer must update it before technical completion when it is within task scope. Quality Engineers and Reviewers may identify stale or missing memory. The Lead Engineer determines whether missing documentation prevents technical completion when the impact is architectural or cross-repository.

Not every task requires documentation changes. If durable documentation remains accurate, no update is required. Do not create documentation merely to record that work occurred; Git history and GitHub operational state preserve execution history.

## Context Loading

For normal Product Work, Repository Engineers must not automatically load all repository documentation. The expected flow is:

Repository Engineer starts -> receives `AGENTS.md` -> identifies authoritative references -> retrieves task-required documentation -> retrieves further context Just-in-Time when necessary

Preserve Least Context, Just-in-Time Context, and artifact-by-reference communication. Agents should follow the actual authoritative repository structure rather than force `src/` or any other convention.

## Security

Repository memory and Codex configuration must never contain passwords, API keys, OAuth tokens, private authentication tokens, cloud credentials, or connection secrets. Do not store them in `AGENTS.md`, `.codex/`, `docs/`, work artifacts, prompts, or repository commits.

External systems must be accessed through approved authenticated capabilities. Secure capability, never credentials.

## Relationship With Existing Governance

This strategy remains consistent with the context and artifact strategy, agent contracts, task lifecycle, execution state, tooling strategy, GitHub MCP governance, orchestration, and Human Gates. Detailed policies remain owned by those documents.

## Initial Strategy Summary

1. `AGENTS.md` is concise, stable, versioned, and repository-specific.
2. `AGENTS.md` is an operating map, not an encyclopedia.
3. `.codex/` contains Codex-specific repository runtime and configuration resources.
4. General technical documentation belongs under root `docs/`, not `.codex/`.
5. `src/` is the preferred source-code convention when compatible with the technology.
6. `docs/works/<work-id>/` stores durable Specifications, Plans, and necessary work artifacts.
7. GitHub, not repository files, owns operational progress.
8. Repository Engineers are specialized by one repository per execution.
9. Multi-repository work uses repository-bounded Atomic Tasks executed sequentially.
10. Cross-repository collaboration uses authoritative artifacts and contracts.
11. Durable facts should have one authoritative home whenever practical.
12. Repository memory changes only when durable technical knowledge changes.
13. Agents retrieve detailed repository context Just-in-Time.
14. Credentials never enter repository memory or Codex project artifacts.

## Deferred Decisions

Do not define yet:

- the exact `.codex/` file structure;
- concrete repository agent configuration files;
- concrete Skill locations;
- mandatory documentation directories for every repository;
- User Story identifier generation;
- automated documentation validation;
- automated stale-document detection.

GitHub Issue hierarchy and Project field/status mapping are defined by [governance/github-workflow.md](github-workflow.md). The remaining decisions belong to later implementation or governance stages.
