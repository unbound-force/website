<!--
  [P] marks tasks eligible for parallel execution.
  Add [P] when a task: (a) touches different files from
  other [P] tasks in the group, (b) has no dependency
  on prior tasks in the group, (c) can safely execute
  without ordering constraints.
  Do NOT add [P] when tasks modify the same file —
  parallel workers will cause merge conflicts.
  Tasks without [P] run sequentially first, then [P]
  tasks run in parallel.
-->

## 1. Update Code Review Tutorial

File: `content/docs/getting-started/code-review-tutorial.md`

- [x] 1.1 Update the command syntax in Step 1 to show the optional PR number argument (`/review-council [N]` or `/review-council code [N]`). Add a brief note explaining that when a PR number is provided, findings can be posted as a GitHub PR review.

- [x] 1.2 Add an "Optional: Post to GitHub" subsection within Step 1 documenting: (a) PR detection via `gh pr view` or explicit PR number, (b) multi-persona aggregation into a single review with per-persona sections, (c) verdict mapping table (APPROVE → `APPROVE`, REQUEST CHANGES → `REQUEST_CHANGES`, APPROVE WITH ADVISORIES → `COMMENT`) with a brief note explaining that APPROVE WITH ADVISORIES occurs when all personas approve but include advisory (LOW-severity) findings, (d) pre-posting safety checks (duplicate review detection, stale review dismissal warnings, CODEOWNER warnings), (e) human confirmation requirement before posting, (f) graceful degradation when `gh` is unavailable or no PR exists.

- [x] 1.3 Add a brief note after the "Complete Loop" ASCII diagram acknowledging that `/review-council N` can optionally post findings to an existing PR (do not modify the diagram itself — the note goes in the existing explanatory paragraph).

- [x] 1.4 Update the decision table to add a row for "Post council findings to a PR" use case (`/review-council N`) and clarify how it differs from `/review-pr N`.

## 2. Update Common Workflows

File: `content/docs/getting-started/common-workflows.md`

- [x] 2.1 [P] Update the "Invoking the Council" subsection to show both `/review-council` (local-only) and `/review-council 42` (with optional GitHub posting) syntax. Add a brief paragraph explaining the optional GitHub posting capability. Also update the "Verdict" subsection to note that when posting to GitHub, the council verdict maps to a GitHub review event (APPROVE → `APPROVE`, REQUEST CHANGES → `REQUEST_CHANGES`, APPROVE WITH ADVISORIES → `COMMENT`).

## 3. Update Developer Guide

File: `content/docs/getting-started/developer.md`

- [x] 3.1 [P] Update any `/review-council` command reference or description to note the optional PR number argument and GitHub posting capability.

## 4. Verification

- [x] 4.1 Run `npm run build` and verify no build errors
- [x] 4.2 Constitution alignment verification: confirm all changes satisfy Content Accuracy (sourced from upstream PR), Minimal Footprint (Markdown-only edits to existing pages), and Visitor Clarity (additions follow existing information architecture)
