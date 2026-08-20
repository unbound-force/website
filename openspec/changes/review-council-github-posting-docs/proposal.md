## Why

GitHub issue [unbound-force/website#179](https://github.com/unbound-force/website/issues/179) documents a new capability added to `/review-council` in [unbound-force/unbound-force#323](https://github.com/unbound-force/unbound-force/pull/323): the council can now optionally post its consolidated review findings as a GitHub PR review.

The website currently describes `/review-council` as a local-only, pre-PR command. With the upstream change, the command now supports:
- An optional PR number argument (`/review-council [N]` or `/review-council code [N]`)
- Posting aggregated multi-persona findings as a single GitHub PR review
- Verdict mapping (APPROVE, REQUEST_CHANGES, COMMENT)
- Pre-posting safety checks (duplicate detection, stale review dismissal, CODEOWNER warnings)
- Human confirmation before posting

The website documentation must be updated to reflect these new capabilities. Without this update, the docs will misrepresent `/review-council` as local-only and omit the new command syntax.

## What Changes

Update three existing documentation pages to document the new GitHub review posting capability of `/review-council`:

1. **Code Review Tutorial** (`content/docs/getting-started/code-review-tutorial.md`) — Add a subsection within Step 1 documenting GitHub review posting ("Optional: Post to GitHub"), update the command syntax to show the optional PR number argument, and revise the decision table.

2. **Common Workflows** (`content/docs/getting-started/common-workflows.md`) — Update the `/review-council` section to note the optional GitHub posting capability and the new PR number argument.

3. **Developer Guide** (`content/docs/getting-started/developer.md`) — Update any command references that describe `/review-council` to note the optional PR posting capability.

## Capabilities

### New Capabilities
- `review-council-github-posting-docs`: Documentation of `/review-council` GitHub PR review posting, including command syntax, verdict mapping, pre-posting checks, and graceful degradation

### Modified Capabilities
- `code-review-tutorial`: Updated with new subsection in Step 1 (GitHub Review Posting), revised command syntax, and updated decision table
- `common-workflows-review-section`: Updated to reflect optional PR posting and new argument syntax
- `developer-guide-commands`: Updated command description for `/review-council`

### Removed Capabilities
- None

## Impact

- **Files modified**: 3 Markdown content pages under `content/docs/getting-started/`
- **No new pages**: All changes are updates to existing documentation
- **No layout/CSS/config changes**: Pure content updates
- **No navigation changes**: Existing menu structure is sufficient
- **Build verification**: `npm run build` must pass after changes

## Constitution Alignment

Assessed against the Unbound Force website constitution (v1.0.0).

### I. Content Accuracy

**Assessment**: PASS

This change directly serves Content Accuracy. The current documentation describes `/review-council` as local-only, which is now inaccurate following the upstream PR [unbound-force#323](https://github.com/unbound-force/unbound-force/pull/323). Updating the docs ensures the website accurately reflects the current state of the tool. All new content is sourced from the implementation PR and its referenced spec artifacts — no capabilities are fabricated.

### II. Minimal Footprint

**Assessment**: PASS

This change modifies only existing Markdown content pages. No new pages, custom HTML, CSS, JavaScript, templates, or dependencies are added. The updates use standard Markdown constructs (tables, code blocks, headings) already present in the affected files.

### III. Visitor Clarity

**Assessment**: PASS

The updates maintain the existing information architecture. The new subsection within Step 1 in the code review tutorial follows the established tutorial pattern. The decision table update helps visitors understand when and how to use the new capability. No new navigation depth is introduced — all content is added to pages visitors already discover through existing navigation paths.
