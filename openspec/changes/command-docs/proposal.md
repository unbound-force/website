## Why

Two significant slash commands — `/review-pr` and `/agent-brief` — have shipped but are not documented on the website. `/review-pr` is the post-PR review counterpart to the already-documented `/review-council`, and `/agent-brief` manages the AGENTS.md lifecycle that is foundational to AI agent effectiveness.

This change addresses GitHub issues #59 (`/review-pr`) and #60 (`/agent-brief`).

## What Changes

1. **`/review-pr` documentation**: Add to the Code Review section of `common-workflows.md` — command description, comparison table with `/review-council`, CI causality analysis explanation, fix branch offering.

2. **`/agent-brief` documentation**: Add to the developer guide or common workflows — command description, create vs audit modes, 5-tier scoring rubric, context-sensitive section detection, bridge file management.

3. **Quick reference table updates**: Add both commands to the quick reference table in `common-workflows.md`.

## Capabilities

### New Capabilities
- `/review-pr` documentation in the Code Review section
- `/agent-brief` documentation for AGENTS.md lifecycle management
- Comparison table: `/review-council` vs `/review-pr` — when to use each

### Modified Capabilities
- `docs/getting-started/common-workflows`: Code Review section expanded, quick reference table updated

### Removed Capabilities
- None

## Impact

- 1 existing page modified (common-workflows.md) — significant additions to Code Review section
- Possibly 1 new page if `/agent-brief` content is large enough to warrant its own page
- Quick reference table updated with 2 new entries

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

Documentation changes — no effect on hero communication.

### II. Composability First

**Assessment**: PASS

Both commands are documented as independently invocable. `/review-pr` works standalone (doesn't require `/review-council` first). `/agent-brief` works in any project (doesn't require Unbound Force).

### III. Observable Quality

**Assessment**: PASS

Documents commands that produce observable, structured output — `/review-pr` produces review findings with severity classifications, `/agent-brief` produces quality scores with a 5-tier rubric.

### IV. Testability

**Assessment**: N/A

No code changes. Validation is `npm run build` + visual review.
