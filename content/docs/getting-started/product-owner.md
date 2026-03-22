---
title: "Getting Started: Product Owner"
description: "Use Muti-Mind for backlog management, priority scoring, and acceptance decisions in the Unbound Force hero workflow."
lead: "Manage the product backlog, score priorities, sync with GitHub Issues, and make acceptance decisions."
date: 2026-03-22T00:00:00+00:00
draft: false
weight: 50
toc: true
---

## Using Muti-Mind

[Muti-Mind](/docs/team/muti-mind/) is the Product Owner hero -- the Vision Keeper and Prioritization Engine. It manages the product backlog, prioritizes work based on business value, guides specification creation, and serves as the acceptance authority for completed increments.

### Backlog Management

Muti-Mind manages backlog items as local Markdown files with YAML frontmatter. Each item includes a title, description, priority, acceptance criteria, and status.

Key operations:

- **Create items**: Add new backlog items with descriptions and acceptance criteria
- **Prioritize**: Score and rank items using the 5-dimension priority engine
- **Generate stories**: Break high-level goals into independent user stories with Given/When/Then acceptance criteria
- **Query**: Search and filter the backlog using the knowledge graph

### Priority Scoring

Every backlog item is scored across 5 dimensions to produce a composite priority (P1 through P5):

| Dimension             | Range        | What It Measures                                        |
| --------------------- | ------------ | ------------------------------------------------------- |
| **Business Value**    | 0-10         | How much value to users and the business                |
| **Risk**              | 0-10         | How much risk is mitigated (higher = reduces more risk) |
| **Dependency Weight** | Bonus        | Boost if the item blocks other items                    |
| **Urgency**           | low-critical | Time sensitivity multiplier                             |
| **Effort**            | XS-XL        | Quick wins (low effort + high value) get a bonus        |

The composite score (0-100) maps to priority levels: P1 (highest urgency) through P5 (backlog). The scoring is transparent -- every priority decision includes a breakdown showing how each dimension contributed.

### Acceptance Decisions

When a feature is complete and validated by Gaze, the PO makes a structured acceptance decision:

- **Accept**: The increment meets all acceptance criteria. Work proceeds to the measure stage.
- **Reject**: The increment fails one or more criteria. A new backlog item is created with the rejection rationale.
- **Conditional**: Partial acceptance with specific conditions that must be met.

### GitHub Issue Sync

Muti-Mind keeps backlog items synchronized with GitHub Issues through bidirectional sync. Items created locally can be pushed to GitHub, and GitHub Issues can be pulled into the local backlog. This keeps the backlog visible to the broader team while maintaining the structured format Muti-Mind needs for priority scoring.

## Your Role in the Hero Lifecycle

The Product Owner participates in two of the six [hero lifecycle stages](/docs/getting-started/common-workflows/#new-feature-end-to-end):

### Stage 1: Define

You start the workflow by creating a backlog item and driving specification creation:

1. Create or select a backlog item with clear acceptance criteria
2. Initiate specification: `/speckit.specify`
3. Participate in clarification: `/speckit.clarify` -- answer questions about scope, user expectations, and edge cases
4. Review the specification before it proceeds to planning

Your acceptance criteria become the standard against which the completed work is evaluated. Write them in Given/When/Then format for clarity:

```
Given a user with a valid session,
When they request their profile data,
Then the response includes their name, email, and preferences.
```

### Stage 5: Accept

After implementation (stage 2), validation (stage 3), and review (stage 4), the increment returns to you for acceptance:

1. Review the **Gaze quality report** -- check contract coverage, CRAP scores, and over-specification
2. Review the **Divisor review verdict** -- check that all 5 personas approved (or understand why they didn't)
3. Compare results against your **acceptance criteria** from stage 1
4. Make your decision: accept, reject, or conditional

If you reject, provide clear rationale. A new backlog item is created automatically with the rejection details so the developer knows exactly what to address.

## Working with Specifications

Speckit specifications are the bridge between your product vision and the developer's implementation plan. Understanding how to read and contribute to specs makes the workflow more effective.

### Reading a Spec

Every spec follows a standard structure:

- **User Stories**: Prioritized descriptions of user journeys with acceptance scenarios (Given/When/Then). Each story is independently implementable and testable.
- **Functional Requirements**: Numbered `FR-NNN` items describing what the system MUST, SHOULD, or MAY do. These are the testable obligations.
- **Success Criteria**: Measurable outcomes like "Users can complete the task in under 2 minutes" or "Coverage exceeds 80%."
- **Edge Cases**: What happens in unusual situations -- boundary conditions, error scenarios, missing data.

### Providing Acceptance Criteria

The most valuable contribution you make to a spec is clear acceptance criteria. Good criteria are:

- **Specific**: "The response includes the user's name" (not "the response is complete")
- **Testable**: Can be verified with a concrete test (not "the UI is intuitive")
- **Independent**: Each criterion can be checked in isolation
- **Structured**: Use Given/When/Then format for unambiguous scenarios

### The Spec Pipeline

Specs flow through a sequential pipeline. You drive the early stages:

1. `/speckit.specify` -- You describe what you want built; the spec is generated
2. `/speckit.clarify` -- You answer questions to reduce ambiguity
3. `/speckit.plan` -- Developer creates the technical plan (you may review)
4. `/speckit.tasks` -- Developer generates the task breakdown
5. `/speckit.implement` -- Developer implements the tasks

## Next Steps

- Read [Common Workflows](/docs/getting-started/common-workflows/) to see how your decisions flow through the full lifecycle
- Explore the [Muti-Mind](/docs/team/muti-mind/) team page for the complete persona details
- Review a [Gaze quality report](/docs/getting-started/tester/#quality-metrics) to understand what you'll evaluate during acceptance
