## Context

The `/review-council` command has gained optional GitHub PR review posting capability via [unbound-force#323](https://github.com/unbound-force/unbound-force/pull/323). Three existing documentation pages need updates to reflect the new command syntax, posting workflow, and verdict mapping. No new pages are required.

The proposal (proposal.md) confirms constitution alignment: all three principles (Content Accuracy, Minimal Footprint, Visitor Clarity) receive PASS. This design preserves that alignment by limiting changes to Markdown content edits within existing page structures.

## Goals / Non-Goals

### Goals
- Document the new `/review-council [N]` command syntax (optional PR number argument)
- Document the GitHub review posting workflow (subsection within Step 1 in the tutorial)
- Document the verdict mapping table (APPROVE → APPROVE, REQUEST CHANGES → REQUEST_CHANGES, APPROVE WITH ADVISORIES → COMMENT)
- Document pre-posting safety checks (duplicate detection, stale review dismissal, CODEOWNER warnings, human confirmation)
- Document graceful degradation (full local-only behavior preserved when `gh` unavailable or no PR exists)
- Update the decision table to reflect the new capability
- Update the `/review-council` vs `/review-pr` comparison to clarify the overlap

### Non-Goals
- Creating a new standalone page for the feature (violates Minimal Footprint)
- Documenting the internal implementation details of review posting (not relevant to website audience)
- Modifying navigation structure or menus
- Updating AGENTS.md governance rules (that document describes pre-PR local review requirements, which remain valid — GitHub posting is an optional addition)
- Documenting Protocol 2 (Issue Linking) — this is an internal implementation detail

## Decisions

### D1: Update existing pages, no new pages

The new capability is an extension of `/review-council`, not a separate feature. Documenting it within the existing Code Review Tutorial, Common Workflows, and Developer Guide pages maintains the current information architecture and avoids page sprawl.

### D2: Add GitHub posting subsection within Step 1 of the Code Review Tutorial

The tutorial currently follows a linear flow: Step 1 (review-council) → Step 2 (push/PR) → Step 3 (review-pr) → Step 4 (causality) → Step 5 (fix branches). The new GitHub posting capability fits as a new section after the existing `/review-council` content in Step 1, since it is an optional extension of the same command. Adding it as a subsection within Step 1 (e.g., "Optional: Post to GitHub") keeps the tutorial flow intact without renumbering all steps.

### D3: Frame as optional capability, not replacement

The documentation must make clear that GitHub posting is opt-in. `/review-council` remains primarily a local pre-PR command. The new capability is for cases where the user wants to post the council's findings to a PR (e.g., when reviewing someone else's PR or when the PR already exists).

### D4: Include the verdict mapping table

The mapping between council verdicts and GitHub review events is important for users to understand what will appear on the PR:

| Council Verdict | GitHub Review Event |
|---|---|
| APPROVE | `APPROVE` |
| REQUEST CHANGES | `REQUEST_CHANGES` |
| APPROVE WITH ADVISORIES | `COMMENT` |

### D5: Document safety checks as a bulleted list

Pre-posting checks (duplicate detection, stale review dismissal warnings, CODEOWNER warnings, human confirmation) are concisely documented as a bulleted list within the tutorial step. This matches the existing documentation style used for safety guards in Step 5.

## Risks / Trade-offs

### Risk: Documentation accuracy depends on upstream PR

The upstream implementation ([unbound-force#323](https://github.com/unbound-force/unbound-force/pull/323)) defines the exact behavior. If the implementation changes before merge, the documentation may need revision.

**Mitigation**: Source all behavioral claims from the implementation PR description and linked spec artifacts. Verify the command syntax and output format against the PR.

### Risk: Overlapping scope with `/review-pr`

With `/review-council` now able to post to GitHub, the distinction between `/review-council` and `/review-pr` becomes less clear-cut. The decision table must be carefully updated to explain when each command is appropriate.

**Mitigation**: Decision D3 frames GitHub posting as optional. The decision table update explicitly addresses the "post council findings to PR" use case as distinct from `/review-pr`'s CI-aware review.

### Trade-off: Subsection vs. new step

Adding the GitHub posting as a subsection within Step 1 avoids renumbering but may make the step longer. A new step would be cleaner but would require renumbering Steps 2-5. The subsection approach is chosen because the feature is optional and contextually belongs with the `/review-council` invocation.
