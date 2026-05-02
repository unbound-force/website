## Why

Two significant slash commands — `/review-pr` and `/agent-brief` — have shipped but are not documented on the website. `/review-pr` is the post-PR review counterpart to the already-documented `/review-council`, and `/agent-brief` manages the AGENTS.md lifecycle that is foundational to AI agent effectiveness.

This change addresses GitHub issues #59 (`/review-pr`) and #60 (`/agent-brief`).

## What Changes

1. **`/review-pr` documentation**: Add to the Code Review section of `common-workflows.md` — command description, comparison table with `/review-council`, CI causality analysis explanation, fix branch offering.

2. **`/agent-brief` documentation**: Add to `common-workflows.md` as a "Project Context Management" section — command description, create vs audit modes, 5-tier scoring rubric, context-sensitive section detection, bridge file management.

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

- 1 existing page modified (`common-workflows.md`) — Code Review section expanded, Project Context Management section added, quick reference table created
- GitHub issues: [#59](https://github.com/unbound-force/website/issues/59), [#60](https://github.com/unbound-force/website/issues/60)

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

Both commands are already shipped and documented in `.opencode/command/*.md` in the `unbound-force/unbound-force` repo. Content will be sourced from the upstream command files (`review-pr.md` and `agent-brief.md`) and cross-referenced with Dewey. Factual claims (5-tier scoring rubric, CI causality analysis, etc.) must be verified against the current shipped state before writing.

### II. Minimal Footprint

**Assessment**: PASS

Adds Markdown content to an existing page (`common-workflows.md`). No custom HTML, CSS, JavaScript, or new dependencies. A quick reference table will be created to improve discoverability — this is standard Markdown with no custom layout.

### III. Visitor Clarity

**Assessment**: PASS

Makes two undocumented commands discoverable. The comparison table (`/review-council` vs `/review-pr`) helps visitors choose the right review command for their situation. The quick reference table provides at-a-glance command discovery. Placement in `common-workflows.md` keeps all workflow commands in one location.
