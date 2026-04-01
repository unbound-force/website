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

- **Accept**: The increment meets all acceptance criteria. Work proceeds to the reflect stage.
- **Reject**: The increment fails one or more criteria. A new backlog item is created with the rejection rationale.
- **Conditional**: Partial acceptance with specific conditions that must be met.

### GitHub Issue Sync

Muti-Mind keeps backlog items synchronized with GitHub Issues through bidirectional sync. Items created locally can be pushed to GitHub, and GitHub Issues can be pulled into the local backlog. This keeps the backlog visible to the broader team while maintaining the structured format Muti-Mind needs for priority scoring.

## Your Role in the Hero Lifecycle

The Product Owner participates in two of the six [hero lifecycle stages](/docs/getting-started/common-workflows/#new-feature-end-to-end):

### Stage 1: Seed

You start the workflow by expressing your intent in 1-2 sentences. This is the **seed** -- a short description of what you want built or fixed. Muti-Mind takes the seed and uses [Dewey](/docs/getting-started/knowledge/) to create the full specification autonomously: retrieving related context from across the organization, drafting the spec with acceptance criteria, self-clarifying using Dewey instead of asking you questions, and validating against historical patterns.

Good seeds are specific enough to convey intent but don't need to be detailed:

- "Fix the authentication timeout bug that users are reporting in issue #142"
- "Add CSV export for the quality dashboard so teams can analyze trends offline"
- "Refactor the webhook handler to support configurable retry logic with exponential backoff"
- "Create a getting-started guide for new contributors to the website project"

The swarm handles everything from here -- specification, planning, implementation, testing, and review -- pausing only at the accept stage for your decision. Developers can run the full pipeline with [`/unleash`](/docs/getting-started/common-workflows/#autonomous-pipeline-unleash), which orchestrates all stages autonomously and exits when human judgment is needed.

To seed a feature, use the `/workflow seed` command:

```text
/workflow seed "Add CSV export for the quality dashboard"
```

This creates a backlog item and starts a workflow with `define=swarm` in one operation. Add `--spec-review` to enable the optional specification review checkpoint. This is equivalent to creating a backlog item and running `/workflow start --define-mode=swarm`.

#### Manual Define Mode

For projects without [Dewey](/docs/getting-started/knowledge/) configured, the define stage runs in manual mode. You drive specification creation directly:

1. Create or select a backlog item with clear acceptance criteria
2. Initiate specification: `/speckit.specify`
3. Participate in clarification: `/speckit.clarify` -- answer questions about scope, user expectations, and edge cases
4. Review the specification before it proceeds to planning

Write acceptance criteria in Given/When/Then format for clarity:

```
Given a user with a valid session,
When they request their profile data,
Then the response includes their name, email, and preferences.
```

#### Optional: Specification Review

For high-stakes features or domains you are unfamiliar with, you can enable a specification review checkpoint. When enabled, Muti-Mind drafts the spec and then pauses for you to scan it before implementation begins. This is a lightweight review -- check for intent alignment and scope accuracy, not implementation details:

- Does the spec address what you actually wanted?
- Is the scope correct -- nothing critical missing, nothing out of scope included?
- Are the acceptance criteria reasonable?

If the spec looks right, approve it and the swarm proceeds. If intent drifted, provide a short correction and Muti-Mind revises. The specification review checkpoint is off by default -- enable it per-workflow when the cost of misalignment outweighs the benefit of full autonomy.

### Stage 5: Accept

After implementation (stage 2), validation (stage 3), and review (stage 4) -- all run autonomously by the swarm -- the workflow pauses and the increment returns to you for acceptance:

1. Review the **Gaze quality report** -- check contract coverage, CRAP scores, and over-specification
2. Review the **Divisor review verdict** -- check that all 5 personas approved (or understand why they didn't)
3. Compare results against your **acceptance criteria** from stage 1
4. Make your decision: accept, reject, or conditional

If you reject, provide clear rationale. A new backlog item is created automatically with the rejection details so the developer knows exactly what to address.

## Knowledge Retrieval with Dewey

When [Dewey](/docs/getting-started/knowledge/) is configured, Muti-Mind uses it to draft specifications autonomously. Dewey's semantic search surfaces related context from across the organization -- past specifications, GitHub issues from other repositories, acceptance criteria patterns, and learning feedback from previous development cycles.

This is what makes the seed interaction possible. Instead of asking you for context about related features or past decisions, Muti-Mind queries Dewey:

- **Cross-repo issue discovery**: Find related bugs and feature requests across all whitelisted repositories, even when they use different terminology
- **Past acceptance criteria**: Surface acceptance scenarios from similar features to inform new specifications
- **Backlog pattern analysis**: Identify recurring themes across the organization's backlog items to spot dependencies and opportunities

When Dewey is not available, Muti-Mind works with the local backlog files and direct GitHub queries. The backlog management workflow is unchanged — Dewey adds broader organizational context but is never required. See the [graceful degradation tiers](/docs/getting-started/knowledge/#graceful-degradation) for details.

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
