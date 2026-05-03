## Why

When CI fails on a pull request, developers face a triage problem: is this failure caused by my changes, or was it already broken? Flaky tests, pre-existing lint violations, and stale dependency warnings create noise that obscures real regressions. Developers waste time investigating failures that predate their PR, or worse, dismiss genuine regressions as "probably pre-existing."

`/review-pr` solves this by fetching CI results and classifying each failure as PR-caused or pre-existing. For pre-existing failures, it offers to create a fix branch. For PR-caused failures, it runs local tools and AI judgment to produce actionable findings. This CI causality analysis pattern is broadly useful — the blog post frames it as a problem any developer faces, not just Unbound Force users.

This change addresses [GitHub issue #64](https://github.com/unbound-force/website/issues/64).

## What Changes

A new blog post at `content/blog/review-pr-causality.md` with the narrative arc:

1. **The problem** — CI failures on PRs: flaky tests, pre-existing violations, wasted investigation time
2. **The solution** — `/review-pr` with CI causality analysis: PR-caused vs pre-existing classification
3. **How it works** — the classification table (base pass/fail × PR pass/fail), the review phases
4. **Fix it forward** — fix branch offering for pre-existing failures (dirty-tree guard, collision check)
5. **The complete review lifecycle** — `/review-council` (pre-PR) + `/review-pr` (post-PR) as complementary commands
6. **In-line PR comments** — 15-comment cap, human confirmation gate

## Capabilities

### New Capabilities
- `blog/review-pr-causality`: Blog post covering CI causality analysis and the review lifecycle

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- 1 new blog post (Markdown content file)
- No layout, style, or configuration changes
- Cross-references to common-workflows docs (where `/review-pr` is documented) and getting-started pages

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

All technical claims (causality classification table, fix branch behavior, 15-comment cap, dirty-tree guard) will be verified against the upstream implementation at `unbound-force/unbound-force/.opencode/command/review-pr.md` at implementation time. The companion angle (review-council vs review-pr) is already documented in `common-workflows.md` (PR #74).

### II. Minimal Footprint

**Assessment**: PASS

Single Markdown blog post file. No custom layouts, CSS, JavaScript, or dependencies.

### III. Visitor Clarity

**Assessment**: PASS

Follows the BA-001 narrative arc. Leads with a universal developer problem ("your CI failed — but was it your fault?") rather than tool-specific framing. The causality classification table gives visitors a concrete mental model. The review lifecycle section shows how the two review commands complement each other.
