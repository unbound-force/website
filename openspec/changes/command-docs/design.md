## Context

The website's `common-workflows.md` is the primary location for slash command documentation. It already covers `/unleash`, `/finale`, and `/review-council` in detail. Two new commands need to be added: `/review-pr` (post-PR review) and `/agent-brief` (AGENTS.md lifecycle). Both are already shipped and documented in `.opencode/command/*.md` in the `unbound-force` repo.

## Goals / Non-Goals

### Goals
- Add `/review-pr` documentation to the Code Review section of `common-workflows.md`
- Add `/agent-brief` documentation to `common-workflows.md` as a "Project Context Management" section
- Create a comparison table for `/review-council` vs `/review-pr`
- Update the quick reference table with both commands
- Document CI causality analysis (PR-caused vs pre-existing failure classification)

### Non-Goals
- Tutorial-style code review walkthrough (tracked in issue #66: Code Review Tutorial)
- Detailed AGENTS.md best practices guide (tracked in issue #65: AGENTS.md Best Practices Blog Post)
- Modifying existing `/review-council` documentation

## Decisions

1. **`/review-pr` in Code Review section**: Natural placement — it's the post-PR counterpart to `/review-council`. Add after the existing `/review-council` content with a comparison table.

2. **`/agent-brief` in common-workflows**: Add as its own section — "Project Context Management". It's a workflow command, not a reference page. Placement in `common-workflows.md` is definitive for this change.

3. **Comparison table from upstream AGENTS.md**: The `unbound-force/unbound-force` repo's AGENTS.md already has a review command comparison table. Adapt it for the website audience (less technical, more "when to use which").

4. **CI causality analysis as a subsection**: This is `/review-pr`'s differentiating feature. Give it a dedicated subsection with a clear explanation of "PR-caused" vs "pre-existing" classification.

## Risks / Trade-offs

- **Risk**: `common-workflows.md` may become too long. The page is currently 465 lines; these additions may push it to ~650-700 lines. If total page length exceeds 700 lines after implementation, extract `/agent-brief` to its own page as a follow-up change. For this change, inline placement is preferred for discoverability.
- **Trade-off**: `/agent-brief` could be a separate page. Chose inline — it's more discoverable in the workflows page and keeps all workflow commands in one place.

## Content Sources

Authoritative upstream source files for documentation content:
- `/review-pr`: `unbound-force/unbound-force/.opencode/command/review-pr.md`
- `/agent-brief`: `unbound-force/unbound-force/.opencode/command/agent-brief.md`
- Comparison table: `unbound-force/unbound-force/AGENTS.md` (review command comparison section)

All documented features (5-tier scoring rubric, CI causality analysis, 15-comment cap, etc.) must be verified against these upstream files at implementation time.
