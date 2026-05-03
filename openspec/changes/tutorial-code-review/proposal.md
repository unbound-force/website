## Why

The website documents `/review-council` and `/review-pr` as reference material in `common-workflows.md` (PR #74) and covers `/review-pr` in a blog post (PR #82). But there is no tutorial that walks an engineer through the complete code review lifecycle — from pre-PR local review to post-PR GitHub review to CI failure triage. Engineers need a step-by-step guide showing when to use each command, what output to expect, and how the two commands complement each other.

This change addresses [GitHub issue #66](https://github.com/unbound-force/website/issues/66).

## What Changes

A new tutorial page at `content/docs/getting-started/code-review-tutorial.md` covering:

1. **Prerequisites**: `uf init` completed, `gh` CLI installed
2. **Pre-PR review with `/review-council`**: Run before pushing — multi-agent Divisor council with Gaze integration
3. **Post-PR review with `/review-pr`**: Run after creating a PR — single-agent review with CI causality analysis
4. **CI causality analysis walkthrough**: Understanding PR-caused vs pre-existing failure classification
5. **Fix branches**: How `/review-pr` offers fix branches for pre-existing failures
6. **The complete loop**: specify → unleash → /review-council → /finale → /review-pr
7. **Decision table**: When to use each command

## Capabilities

### New Capabilities
- `docs/getting-started/code-review-tutorial`: Tutorial page for the complete code review lifecycle

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- 1 new tutorial page (Markdown content)
- No layout, style, or configuration changes
- Cross-references to common-workflows docs (comparison table) and blog posts

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

All commands, flags, and output examples will be verified against the upstream implementation at `unbound-force/unbound-force/.opencode/command/review-council.md` and `review-pr.md` at implementation time. The decision table maps directly to the commands' actual behavior.

### II. Minimal Footprint

**Assessment**: PASS

Single Markdown tutorial page under the existing Getting Started section. No custom layouts, CSS, JavaScript, or dependencies. Uses Doks built-in sidebar navigation.

### III. Visitor Clarity

**Assessment**: PASS

Step-by-step tutorial structure with expected output examples. The decision table at the end provides a quick-reference for returning visitors. The "complete loop" section shows how review commands fit into the overall development workflow.
