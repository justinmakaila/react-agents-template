# Product Owner

## Summary

The Product Owner is responsible for defining the vision, goals, and priorities for the system. They ensure that the work being done aligns with user needs, business objectives, and strategic direction. The Product Owner represents the "why" behind features and tasks.

---

## Responsibilities

- Define the overall product direction and vision.
- Translate user needs or business objectives into clear requirements.
- Maintain and prioritize the product roadmap or backlog.
- Validate that delivered work meets expectations and intended outcomes.
- Make trade-off decisions between scope, quality, and timeline.
- Provide clarifications when technical or design questions arise.

---

## Inputs

- User feedback, goals, and external requirements.
- Strategic or business objectives.
- Technical feasibility reports from engineering.
- System constraints or upcoming platform limitations.

---

## Outputs

- Requirements, user stories, or acceptance criteria.
- Prioritized lists of features or tasks.
- Approval or rejection of completed work.
- Clarifications to unblock engineering or AI agents.

---

## Interaction Patterns

### With Planner Agent

- Provides high-level goals or user stories.
- Reviews structured plans and confirms priorities.
- Clarifies ambiguous requirements.

### With Codegen Agent

- Rare direct interaction; typically operates through the Planner or Staff Engineer.
- May review generated documentation or prototypes.

### With Reviewer Agent

- Approves changes from a product perspective (not code quality).

### With Staff Engineer

- Discusses feasibility, constraints, and sequencing of work.
- Participates in shaping architectural decisions when necessary.

---

## Decision Boundaries

The Product Owner:

- Owns **what** should be built.
- Does **not** dictate **how** it is implemented.
- Can reprioritize or modify goals based on changing needs.

---

## Examples

### Example Directive

“Users need to filter courses by proximity; this should load quickly and work offline.”

### Example Clarification

“The distance value should be calculated from the user’s last known location, not live GPS unless available.”
