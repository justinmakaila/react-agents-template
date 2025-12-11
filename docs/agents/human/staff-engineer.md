# Staff Engineer

## Summary

The Staff Engineer is responsible for the technical direction and architecture of the system. They ensure designs are maintainable, scalable, and coherent. They act as the technical authority for complex decisions and guide contributors (human or AI) toward sound solutions.

---

## Responsibilities

- Define and maintain the system’s architectural principles and boundaries.
- Make high-impact technical decisions and establish design patterns.
- Review complex or high-risk changes.
- Support the Planner Agent in structuring technically coherent tasks.
- Provide guidance, context, and mentorship to contributors.
- Ensure the system remains performant, maintainable, and secure.

---

## Inputs

- Requirements, goals, and constraints from the Product Owner.
- Technical insights from past work, system behavior, and project history.
- Feedback from reviewers, planners, and code generators.
- Operational or performance data when available.

---

## Outputs

- Architectural guidance, decisions, and diagrams.
- Design proposals or technical specifications.
- Clarifications for planners and implementers.
- Reviews of complex technical work.

---

## Interaction Patterns

### With Planner Agent

- Ensures plans are technically feasible and logically staged.
- Provides necessary context or constraints.
- Helps refine vague or overly broad tasks.

### With Codegen Agent

- Provides instructions for preferred patterns or design principles.
- Reviews complex code or system integrations.
- Flags risks or inconsistencies early.

### With Reviewer Agent

- Partners to evaluate correctness, maintainability, and long-term implications.
- May overrule or refine reviewer feedback when architectural concerns are involved.

### With Product Owner

- Translates product goals into technical milestones.
- Identifies technical debt or platform constraints affecting roadmap.

---

## Decision Boundaries

The Staff Engineer:

- Owns **how** the system is architected and implemented.
- Does **not** override the Product Owner’s **what** unless requirements are infeasible.
- Can block changes that violate architectural integrity.

---

## Examples

### Example Direction

“To avoid state drift, the telemetry module must use a single source-of-truth event stream.”

### Example Clarification

“Pagination should use cursor-based navigation; offset pagination won’t scale for millions of records.”
