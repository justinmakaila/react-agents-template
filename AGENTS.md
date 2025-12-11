# Agents

This repo defines the following agents:

## Human Agents

- [Product Owner](docs/agents/human/product-owner.md) – Owns feature roadmap and acceptance criteria.
- [Staff Engineer](docs/agents/human/staff-engineer.md) – Owns architecture decisions and code reviews.

## AI Agents

- [Planner](docs/agents/ai/planner-agent.md) – Breaks goals into tasks, routes to other agents.
- [Codegen](docs/agents/ai/codegen-agent.md) – Writes and edits code given specs and context.
- [Reviewer](docs/agents/ai/reviewer-agent.md) – Reviews diffs, checks for regressions and style issues.

## Systems

The following define how specific systems or components should be defined and structured within the project:

- [React Architecture](docs/agents/systems/react/architecture.md) - React architecture overview
  - [Components](docs/agents/systems/react/components.md) - React presentation components
  - [Containers](docs/agents/systems/react/containers.md) - React "smart" components
  - [Contexts](docs/agents/systems/react/contexts.md) - React contexts.
