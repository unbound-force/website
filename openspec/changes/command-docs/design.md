## Context

The website's `common-workflows.md` is the primary location for slash command documentation. It already covers `/unleash`, `/finale`, and `/review-council` in detail. Two new commands need to be added: `/review-pr` (post-PR review) and `/agent-brief` (AGENTS.md lifecycle). Both are already shipped and documented in `.opencode/command/*.md` in the `unbound-force` repo.

## Goals / Non-Goals

### Goals
- Add `/review-pr` documentation to the Code Review section of `common-workflows.md`
- Add `/agent-brief` documentation (location TBD — may be in common-workflows or a separate page)
- Create a comparison table for `/review-council` vs `/review-pr`
- Update the quick reference table with both commands
- Document CI causality analysis (PR-caused vs pre-existing failure classification)

### Non-Goals
- Tutorial-style code review walkthrough (covered by tutorial #66)
- Detailed AGENTS.md best practices guide (covered by blog #65)
- Modifying existing `/review-council` documentation

## Decisions

1. **`/review-pr` in Code Review section**: Natural placement — it's the post-PR counterpart to `/review-council`. Add after the existing `/review-council` content with a comparison table.

2. **`/agent-brief` in common-workflows**: Add as its own section — "Project Context Management" or similar. It's a workflow command, not a reference page. If it grows too large, it can be extracted to its own page later.

3. **Comparison table from AGENTS.md**: The `unbound-force` AGENTS.md already has a review command comparison table. Adapt it for the website audience (less technical, more "when to use which").

4. **CI causality analysis as a subsection**: This is `/review-pr`'s differentiating feature. Give it a dedicated subsection with a clear explanation of "PR-caused" vs "pre-existing" classification.

## Risks / Trade-offs

- **Risk**: `common-workflows.md` may become too long. Current length is manageable, and these additions are proportionate to existing sections. Monitor.
- **Trade-off**: `/agent-brief` could be a separate page. Chose inline for now — it's more discoverable in the workflows page, and the content is ~200 lines.
