# Initial Agent Architecture

## Core Distinction

Roles != Agents != Workflow Steps

A role defines responsibility. An agent exists only when isolated execution, specialized context, independent judgment, or delegation provides meaningful value. Agents are not created merely to imitate a traditional company organization.

## Agent Hierarchy

The initial hierarchy is:

- Level 0 - Human Operator
- Level 1 - Root Engineering Copilot
- Level 2 - Lead Engineer / Architect
- Level 3 - Specialist Agents

Normal delegation stops at Level 3. Specialist agents must not recursively create additional specialist hierarchies. If another competency is required, the specialist reports that need to the Lead Engineer, which decides how to route it. Delegation therefore remains bounded, traceable, and cost-aware.

## Central Agents

### Root Engineering Copilot

The Root Engineering Copilot is a dedicated root agent. Its responsibilities are defined in [governance/copilot.md](copilot.md). It owns product coordination, global orchestration, state reconstruction, context routing, human escalation, and consolidated communication with the human operator. It must not become the primary technical implementation agent.

### Lead Engineer / Architect

The Lead Engineer / Architect is a dedicated technical orchestration agent. It receives approved product requirements and performs engineering analysis, including:

- technical specifications;
- architecture impact analysis;
- affected repositories and components;
- contracts and dependencies;
- technical risks;
- implementation and testing strategies;
- decomposition into atomic engineering tasks;
- selection and orchestration of required technical specialists.

It acts as the technical manager of an approved engineering effort and may delegate isolated tasks to Level 3 specialists.

## Repository Engineers

Implementation engineers are primarily specialized by repository rather than represented by one globally generic developer agent. Examples include frontend, backend, mobile, platform, and Strapi/CMS engineers operating in the repositories where those competencies apply.

Repository specialization comes from local context such as root `AGENTS.md`, `.codex/`, repository documentation, technical specifications, task artifacts, source code, and tests. The repository should contain the knowledge required to work correctly in that project. A Repository Engineer receives only the context required for the current atomic task.

## Independent Specialist Agents

### Code Reviewer

A dedicated independent execution context is used for code review. Inputs may include the technical specification, acceptance criteria, repository rules, code diff, tests, and validation evidence. The reviewer must not inherit the implementation agent's private execution context.

Its purpose is independent evaluation of correctness, maintainability, bugs, regressions, unnecessary complexity, repository rule violations, missing tests, and specification inconsistencies.

### Quality Engineer

The Quality Engineer is a generic independent specialist whose project-specific testing knowledge comes from the repository being evaluated. It determines whether sufficient evidence shows that the implementation satisfies the product requirement and technical specification.

It may evaluate coverage, identify missing scenarios and edge cases, verify acceptance criteria, inspect deterministic test results, add or request additional automated tests when appropriate, and produce concise manual verification checklists only when the Human requests them. Repository-specific testing instructions belong near the project, such as in testing documentation, rather than in this generic role.

The implementation engineer remains responsible for implementation-related tests and relevant local deterministic checks. The Quality Engineer must not become a second implementation engineer. A production-code defect is reported with evidence through the engineering effort rather than fixed directly by the Quality Engineer. During the initial phase, manual human verification is acceptable for frontend visual or navigation validation when browser automation would consume disproportionate effort or model budget.

### Security Reviewer

A dedicated, on-demand independent specialist used for meaningful security impact, including authentication, authorization, public exposure, file uploads, secrets, permissions, sensitive data, infrastructure, and security-critical endpoints. It is not invoked for every task.

## Capabilities Without Separate Agents

### Architecture Review

No separate Architecture Reviewer agent is created. When independent architecture review is required, the Lead Engineer / Architect capability is reused in a fresh isolated execution context. The reviewing execution must not inherit the private context used to create the original technical design.

### CMS and Database Specialization

No dedicated CMS Engineer or Database Engineer agent is created at this stage. CMS expertise, such as Strapi, comes primarily from repository specialization, authoritative documentation, and explicit Just-in-Time retrieval. An optional Skill may contribute only when one has been adopted on demonstrated evidence. Database responsibilities belong to Backend Engineering and Lead Engineer / Architecture responsibilities, supplemented by project-specific knowledge when required.

### UX/UI Design

UX/UI design is an on-demand role, not a permanent V1 agent. When approved work requires product-specific design expertise, the Lead Engineer may request an appropriate isolated specialist execution. No generic UX/UI agent definition is established in the initial specialist set.

### Organizational Roles

No dedicated agents are created at this stage for Scrum Master, Product Owner, Release Manager, or Technical Writer. Their useful responsibilities are covered by the human operator, Root Engineering Copilot, deterministic project workflows, Platform Engineering, and the agents responsible for producing and maintaining their own artifacts.

## Delegation Model

The normal technical delegation model is:

Human Operator -> Root Engineering Copilot -> Lead Engineer -> Specialist Agent

Specialist executions remain isolated. The Lead Engineer receives structured results and persistent artifacts rather than the full private reasoning context of specialists. The Root Engineering Copilot receives consolidated engineering results rather than all intermediate specialist context.

## Cost Principle

The existence of an agent does not imply participation in every task. Agent participation remains proportional to task complexity and risk.

Examples:

- A trivial administrative task may require only Copilot -> deterministic verification -> done.
- A small repository task may require Lead Engineer -> Repository Engineer -> deterministic verification.
- A higher-risk task may additionally require Code Reviewer, Quality Engineer, Security Reviewer, or an approved on-demand specialist according to the work.
