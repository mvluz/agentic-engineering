# Skills Strategy

This document defines when Codex Skills should and should not be used in the initial Agentic Engineering system. Skills are optional capabilities, not a mandatory part of the runtime.

## Core Principle

Agentic Engineering is documentation-first and explicit-routing-first.

Do not create a Skill merely because Codex supports Skills. A Skill should exist only when it provides a demonstrated operational benefit over authoritative documentation and explicit workflow routing.

## Documentation First

Engineering procedures should initially live in durable, versioned, authoritative documentation. Depending on ownership, procedures may belong in repository `docs/`, Agentic Engineering governance, or other approved durable documentation.

The applicable Repository Agent Strategy determines repository documentation structure. Procedures may cover development, migration, testing, deployment, architecture, or framework engineering.

## AGENTS.md as Procedure Map

`AGENTS.md` should remain concise. It may state when an agent must consult authoritative documentation, but it must not copy the complete procedure.

AGENTS.md -> tells the agent where and when to look

docs/ -> contains the authoritative detailed procedure

## Explicit Procedure Routing

Normal autonomous workflows must not depend on Skill discovery. Workflow rules, the Lead Engineer, Repository Engineer instructions, or applicable repository guidance should identify the required procedure.

workflow stage -> identifies applicable procedure -> retrieves it Just-in-Time -> executes work

Prefer this explicit routing over exposing metadata for many possible Skills and asking an agent to select among them.

## Just-in-Time Procedure Loading

Do not load procedure documentation into every Agent Run merely because it exists. A Repository Engineer changing validation logic does not need migration procedures; an Engineer creating a migration may retrieve the migration procedure when required.

Preserve Least Context, Just-in-Time Context, and explicit procedure routing. This keeps each execution focused and reduces unnecessary context and capability awareness.

## No Repository Skills by Default

Product repositories do not require Skills merely because they use a particular technology or procedure, including Flyway, PostgreSQL, React, Next.js, Strapi, Docker, React Native, or Expo.

Repository-specific procedures should normally begin as `AGENTS.md` plus authoritative `docs/`. A repository Skill may be introduced later only when documentation and explicit routing do not provide the required benefit sufficiently.

## Skills Are Not Memory, State, Roles, or External Capabilities

Skills must not substitute for repository memory. Repository facts belong in `AGENTS.md`, `docs/`, authoritative contracts, architecture and testing documentation, and work-specific artifacts.

Skills must not store current tasks, next tasks, blockers, execution progress, GitHub state, token usage, or Feature state. Operational work state belongs in GitHub Issues / GitHub Projects.

Skills must not substitute for Root Engineering Copilot, Lead Engineer, Repository Engineer, Quality Engineer, Reviewer, or other approved roles. A Skill is a procedure or capability package, not an autonomous authority.

Skills must not substitute for authenticated external capabilities. GitHub uses MCP, AWS uses an approved authenticated capability, and Figma uses MCP or another approved external capability. A Skill may explain how to use an approved capability but must not contain or expose credentials.

Preserve: Secure capability, never credentials.

## Skill Metadata Cost

Skills may expose discovery metadata even when their full content is not loaded. Unnecessary Skills can therefore increase initial context and capability awareness. Do not maintain large catalogs merely for convenience when explicit procedure references are sufficient.

## Evidence-Based Adoption

Use this progression:

authoritative documentation -> explicit routing -> repeated real-world use -> observed friction or duplication -> evaluate Skill value -> create Skill only if justified

The initial number of custom Agentic Engineering Skills may legitimately be zero.

Skills may provide value when one or more of the following is demonstrated:

- a Human-facing shortcut improves recurring interaction;
- a procedure is broadly reusable outside a deterministic workflow;
- scripts, templates, or resources benefit from being packaged together;
- runtime discovery is genuinely needed;
- cross-repository reuse is cumbersome through documentation alone.

These are candidate conditions, not automatic reasons to create a Skill.

## One Authoritative Procedure

When a procedure gains a Skill-based shortcut or wrapper, avoid competing authoritative implementations. For example, an optional `/flyway` Skill may reference an authoritative `docs/.../flyway-migration.md` procedure.

The Skill may provide ergonomic invocation or package supporting resources, while the documentation remains the engineering source of truth. If packaging requires moving the procedure into a Skill, that migration must deliberately redefine the authoritative source rather than silently duplicate it.

## Thin Shortcuts and Autonomous Workflows

A useful Skill may be intentionally thin:

Skill: `/flyway`

Purpose: invoke the approved Flyway migration workflow.

Authoritative procedure: <reference>

The autonomous framework should receive procedure decisions through workflow rules, Agent Contracts, repository instructions, Technical Specifications, Implementation Plans, Atomic Task context, and explicit routing. It should not rely on broad Skill discovery.

The Root Engineering Copilot and Lead Engineer do not require Skills merely because they coordinate many workflows. Their normal procedures should initially be encoded through governance, agent instructions, workflow documentation, and artifact references. Human-facing shortcuts may later be useful, but are not required in v1.

## Procedures Versus Skills

Prefer authoritative documentation when the workflow already knows when the procedure applies, the procedure is repository-specific, explicit retrieval is simple, only instructions are required, discovery adds little value, or minimizing preload context matters.

Consider a Skill when Human invocation benefits from a shortcut, the workflow is reused broadly outside a deterministic path, packaging scripts or resources provides real value, dynamic discovery is useful, or cross-repository reuse becomes cumbersome through documentation alone.

When both approaches work equally well, prefer authoritative documentation and explicit routing.

## Security

Skills must follow all Agentic Engineering security governance. Never place credentials, API keys, OAuth tokens, passwords, cloud secrets, or private authentication material in Skill instructions, scripts, resources, examples, or prompts.

A Skill may operate only with capabilities already granted to the Agent Run. A Skill does not grant authority or permission by itself.

## Relationship With Repository and Context Strategy

Repository procedures remain consistent with `governance/repository-agent-strategy.md`:

- `AGENTS.md`: essential map and procedure references;
- `docs/`: detailed authoritative procedures and repository knowledge;
- Skills: optional optimization when proven useful.

Skills Strategy preserves Least Context, Just-in-Time Context, artifact-by-reference communication, explicit context routing, and minimal capability exposure. The preferred execution model is:

workflow -> determines required procedure -> retrieves only that procedure -> executes isolated Agent Run

## Initial V1 Policy

Agentic Engineering v1 must operate successfully without custom Skills. Core operation depends on governance, agent instructions, repository `AGENTS.md`, authoritative documentation, explicit procedure routing, Just-in-Time retrieval, approved MCP capabilities, and deterministic tooling where appropriate.

Skills may be added later without changing the fundamental architecture, but adoption must be based on evidence from real workflows rather than speculation.

## Initial Strategy Summary

1. Agentic Engineering is documentation-first and explicit-routing-first.
2. Procedures are loaded Just-in-Time.
3. Skills are optional and not mandatory in v1.
4. Repositories do not receive Skills by default.
5. Skills are not repository memory, operational state, agent roles, or external authenticated capabilities.
6. Avoid unnecessary Skill metadata in agent context.
7. Prefer documentation when procedure applicability is already known.
8. Use Skills only for demonstrated ergonomic, packaging, reuse, scripting, or discovery value.
9. Human-facing shortcuts are a valid candidate use case.
10. Avoid duplicating authoritative procedures between documentation and Skills.
11. The initial custom Skill count may be zero.
12. Adopt Skills based on evidence from real workflows.

## Relationship With Existing Governance

Skills Strategy remains consistent with:

- `governance/principles.md`;
- `governance/tooling-strategy.md`;
- `governance/context-artifact-strategy.md`;
- `governance/repository-agent-strategy.md`;
- `governance/agent-contracts.md`;
- `governance/execution-budget.md`.

Detailed policies remain owned by those documents.

## Deferred Decisions

Do not define yet:

- concrete Skills;
- Skill directory structures;
- Skill installation mechanisms;
- Skill discovery configuration;
- custom Skill scripts;
- Human shortcut syntax;
- repository-local Skills;
- automatic Skill selection policies.

A Skill-specific technical spike is not mandatory unless a real Skill requirement is introduced.
