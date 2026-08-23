# Engineering Role Catalog

This document defines roles required by the Agentic Engineering system. Roles represent responsibilities; they do not imply independent agents. The decision about which roles require dedicated agents is deferred.

## Product Layer

### Human Product Authority

The human operator is the final authority. This role defines product goals, priorities, desired behavior, scope, business expectations, and important product decisions. The human also performs the final engineering validation after the workflow completes.

### Root Engineering Copilot

The Root Engineering Copilot is defined in [governance/copilot.md](copilot.md). In this layer, it provides product coordination, orchestration, context routing, and persistent-state coordination.

## Engineering Layer

### Lead Engineer / Architect

Translates approved product requirements into engineering direction. Responsibilities may include technical analysis, technical specifications, architecture impact, affected components, API and integration contracts, data model impact, dependencies, migration impact, technical risks, implementation strategy, testing strategy, and decomposition into atomic engineering tasks.

This role answers: “How should this requirement be engineered?”

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

### QA / Test Engineer

Responsible for validating that implementation satisfies product requirements and technical specifications. Responsibilities include test strategy, acceptance scenarios, edge cases, regression scenarios, functional validation, integration validation, and verification of acceptance criteria.

### Code Reviewer

Responsible for independent implementation review. Typical inputs include the relevant specification, code diff, tests, and required repository context. Review concerns include correctness, bugs, maintainability, readability, unnecessary complexity, repository rule violations, regressions, missing tests, and inconsistencies with the specification. Independent context should be preferred when this role is used.

### Architecture Reviewer

An on-demand role for work with meaningful architectural impact, such as cross-repository changes, new integrations, new major components, architecture changes, or changes affecting multiple systems. This role may later reuse the Architect capability in an independent execution context rather than becoming a separate permanent agent.

### Security Reviewer

An on-demand role for work with meaningful security impact, including authentication, authorization, file uploads, public exposure, secrets, infrastructure, sensitive data, permissions, and security-critical endpoints. This role is not required for every task.

## Roles Not Currently Required

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

A role represents a responsibility that must exist in the engineering system. Whether that responsibility requires a dedicated agent, reusable specialist capability, skill, fresh execution context, deterministic automation, or another existing agent will be decided separately.

