## Context

The website has four factual errors or stale data points in its getting-started documentation. These were identified through cross-referencing shipped command behavior (in `unbound-force/unbound-force`) against the website's descriptions. All four are text-only corrections — no layout, template, or configuration changes.

## Goals / Non-Goals

### Goals
- Correct all four factual errors in a single change
- Ensure corrected text matches the shipped command behavior (source of truth: `.opencode/command/*.md` in the `unbound-force` repo)
- Preserve page structure, frontmatter, and surrounding content

### Non-Goals
- Restructuring the getting-started section
- Adding new documentation pages
- Updating command reference tables (covered by separate CLI reference change)
- Addressing other stale content not identified in issues #53-#56

## Decisions

1. **Bundle four fixes into one change**: All four are small, non-overlapping text corrections. Separate branches per fix would add overhead without benefit.

2. **Source of truth is shipped commands, not specs**: Issue #53 is notable — the `/finale` spec (opsx/finale-command) includes a `finale-merge` requirement, but the shipped command at `.opencode/command/finale.md` explicitly says "NEVER merge the PR." The shipped behavior is the source of truth for documentation.

3. **Exact replacement text from issues**: Each issue provides explicit before/after text. Use the issue-provided corrections verbatim rather than rephrasing.

## Risks / Trade-offs

- **Risk**: The `/finale` spec and shipped command disagree on merge behavior. If the spec is later re-implemented, the docs would need updating again. Accepted — we document shipped behavior, not planned behavior.
- **Risk**: File count (#55) may drift again as commands are added/removed. Accepted — use "approximately" language and update the breakdown table to match current state.
