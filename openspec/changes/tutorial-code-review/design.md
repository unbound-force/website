## Context

The website has reference documentation for both review commands in `common-workflows.md` (PR #74) and a blog post on `/review-pr` CI causality (PR #82). Issue #66 requests a tutorial that walks through the complete lifecycle. Tutorials differ from reference docs (command surface) and blog posts (narrative arc) — they are step-by-step guides with expected output at each stage.

## Goals / Non-Goals

### Goals
- Create a step-by-step tutorial covering both review commands in sequence
- Show expected output at each step (fenced `text` code blocks)
- Include the decision table for quick command selection
- Show the commands in the context of the full development loop
- Cover CI causality analysis with a concrete example

### Non-Goals
- Duplicate the full command reference (flags, phases) — link to common-workflows docs
- Duplicate the blog post's narrative on CI causality — link to the blog post
- Cover Gaze integration in depth (optional feature, not required for the tutorial)
- Cover review council configuration or Divisor agent customization

## Decisions

1. **Page location**: `content/docs/getting-started/code-review-tutorial.md` — tutorials belong in Getting Started, not Reference. Weight after `common-workflows.md` to position it as a follow-up.

2. **Structure**: Linear walkthrough — prerequisites → review-council → push & PR → review-pr → causality → fix branch → complete loop → decision table. Each section has a "Run" step and an "Expected output" block.

3. **Expected output style**: Use fenced `text` code blocks showing representative (not exact) output. Note that actual output varies by project.

4. **Decision table at the end**: The issue's decision table is the quick-reference payoff. Place it at the end as a summary the reader can bookmark.

5. **Frontmatter**: Standard docs page frontmatter (title, description, lead, date, weight, toc). Not a blog post — no categories/tags/contributors.

6. **Cross-references**: Link to common-workflows (comparison table), blog post (narrative depth), and getting-started pages.

## Content Sources

Authoritative upstream sources:
- `/review-council`: `unbound-force/unbound-force/.opencode/command/review-council.md`
- `/review-pr`: `unbound-force/unbound-force/.opencode/command/review-pr.md`
- Comparison table: `content/docs/getting-started/common-workflows.md` (if PR #74 merged)

All commands and output examples must be verified against the upstream command files at implementation time.

## Risks / Trade-offs

- **Risk**: The tutorial may go stale as review commands evolve. Mitigated by keeping output examples generic and linking to reference docs for exact behavior.
- **Trade-off**: Depth vs. length. Chose moderate depth — enough to follow along, not enough to be a reference manual. The decision table provides the quick-hit value for readers who do not want the full walkthrough.
