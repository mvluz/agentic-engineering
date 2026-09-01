# Engineering Role Catalog

This document defines engineering responsibilities. Roles do not automatically imply permanent framework agents. The approved V1 agent definitions are Root Engineering Copilot, Lead Engineer / Architect, Repository Engineer, Code Reviewer, and on-demand Quality Engineer and Security Reviewer; their operational contracts are defined under `agents/`.

## Product Layer

### Human Product Authority

The human operator is the final authority. This role defines product goals, priorities, desired behavior, scope, business expectations, and important product decisions. The human also performs the final engineering validation after the workflow completes.

### Root Engineering Copilot

The Root Engineering Copilot is defined in [governance/copilot.md](copilot.md). In this layer, it provides product coordination, orchestration, context routing, and persistent-state coordination.

## Engineering Layer

### Lead Engineer / Architect

Translates approved product requirements into engineering direction. Responsibilities may include technical analysis, technical specifications, architecture impact, affected components, API and integration contracts, data model impact, dependencies, migration impact, technical risks, implementation strategy, testing strategy, and decomposition into atomic engineering tasks.

This role answers: “How should this requirement be engineered?”

### Repository Engineer

Implements one Atomic Task in one repository. Repository context determines practical specialization and durable repository memory remains close to that repository.

### Frontend Engineer

Responsible for web frontend engineering, including UI implementation, reusable components, state management, API and CMS integration, accessibility, frontend performance, responsive behavior, frontend tests, and adherence to the project design system. Frontend specialization is project- or repository-specific.

### Backend Engineer

Responsible for APIs, business rules, contracts, persistence, integrations, authentication and authorization when applicable, database migrations, and backend tests. Backend specialization is project- or repository-specific.

### Mobile Engineer

Responsible for mobile-specific engineering, including mobile application architecture, navigation, local storage, offline behavior, native integrations, application lifecycle, API integration, mobile tests, and build and distribution concerns when applicable. This role participates only in projects containing a mobile application.

### Platform Engineer

Responsible for the shared runtime and delivery platform, including Docker, Docker Compose, VPS infrastructure, CI/CD, GitHub Actions, gateways and routing, Cloudflare, networking, deployment, observability, backups, secret-management integrations, and runtime operations. The role is named Platform Engineer rather than being limited to DevOps terminology.

### UX/UI Designer

An on-demand role responsible for user experience, visual hierarchy, interaction design, responsive design, accessibility decisions, design systems, component behavior, and interface consistency. This role is not assumed to require a dedicated agent.

## Validation Layer

### Quality Engineer

An on-demand role that evaluates whether evidence sufficiently demonstrates required behavior. The Repository Engineer retains responsibility for implementation-related testing and deterministic verification.

### Code Reviewer

Responsible for independent implementation review. Typical inputs include the relevant specification, code diff, tests, and required repository context. Review concerns include correctness, bugs, maintainability, readability, unnecessary complexity, repository rule violations, regressions, missing tests, and inconsistencies with the specification. Independent context should be preferred when this role is used.

### Architecture Reviewer

An on-demand responsibility for work with meaningful architectural impact, such as cross-repository changes, new integrations, new major components, architecture changes, or changes affecting multiple systems. It reuses the Lead Engineer / Architect capability in a fresh isolated execution context rather than requiring a separate permanent agent.

### Security Reviewer

An on-demand role for work with meaningful security impact, including authentication, authorization, file uploads, public exposure, secrets, infrastructure, sensitive data, permissions, and security-critical endpoints. This role is not required for every task.

## No Separate V1 Agent Definition

The following are intentionally not introduced as dedicated roles at this stage:

- Scrum Master;
- separate Product Owner agent;
- Database Engineer;
- Release Manager;
- Technical Writer;
- dedicated CMS Engineer.

Scrum and process coordination can initially be handled by the Copilot, GitHub Projects, and deterministic workflows. Product authority remains with the human, supported operationally by the Copilot. Database responsibilities currently fit within Backend and Architecture roles, while release responsibilities fit within Platform Engineering and delivery workflows. Documentation should be maintained by the roles responsible for the work. CMS specialization may later become a skill or Backend Engineering specialization rather than a dedicated role.

## Core Rule

Roles != Agents != Workflow Steps

A role represents a responsibility that must exist in the engineering system. Frontend, Backend, Mobile, Platform, CMS, database, and similar responsibilities remain repository or task specializations unless approved governance establishes a separate agent definition.
