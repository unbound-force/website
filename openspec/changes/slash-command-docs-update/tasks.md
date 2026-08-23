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

## 1. Namespace Migration — Documentation Pages (#220)

Update all hero-specific slash command references to the `uf.*` namespace in documentation pages. Each task touches a different file so all are parallel-eligible.

- [x] 1.1 [P] Update `content/docs/getting-started/common-workflows.md`: Replace all `/unleash` → `/uf.unleash`, `/finale` → `/uf.finale`, `/cobalt-crush` → `/uf.cobalt-crush`, `/review-council` → `/uf.review-council` references. Preserve heading anchor slugs by using Hugo `{#anchor-id}` syntax where headings are referenced by internal links (e.g., keep `{#autonomous-pipeline-unleash}` anchor on the renamed heading). Add new sections for `/uf.triage-issue`, `/forge`, `/forge:status`, `/org`, `/inbox`, `/handoff` commands.
- [x] 1.2 [P] Update `content/docs/getting-started/developer.md`: Replace all `/unleash` → `/uf.unleash`, `/finale` → `/uf.finale`, `/cobalt-crush` → `/uf.cobalt-crush` references. Distinguish persona name "Cobalt-Crush" (unchanged) from command `/uf.cobalt-crush` (updated).
- [x] 1.3 [P] Update `content/docs/getting-started/quick-start.md`: Replace `/unleash` → `/uf.unleash`, `/finale` → `/uf.finale`, `/cobalt-crush` → `/uf.cobalt-crush` in code examples and prose.
- [x] 1.4 [P] Update `content/docs/getting-started/code-review-tutorial.md`: Replace all `/review-council` → `/uf.review-council`, `/review-pr` → `/uf.review-pr`, `/finale` → `/uf.finale`, `/unleash` → `/uf.unleash` references throughout the tutorial.
- [x] 1.5 [P] Update `content/docs/getting-started/tester.md`: Replace `/review-council` → `/uf.review-council`.
- [x] 1.6 [P] Update `content/docs/getting-started/product-owner.md`: Replace `/unleash` → `/uf.unleash`.
- [x] 1.7 [P] Update `content/docs/getting-started/architecture.md`: Replace `/unleash` → `/uf.unleash`, `/review-council` → `/uf.review-council`, `/cobalt-crush` → `/uf.cobalt-crush` references. Keep persona name references unchanged.
- [x] 1.8 [P] Update `content/docs/getting-started/_index.md`: Replace `/unleash` → `/uf.unleash`, `/cobalt-crush` → `/uf.cobalt-crush` references. Keep persona name "Cobalt-Crush" unchanged.
- [x] 1.9 [P] Update `content/docs/getting-started/constitution.md`: Replace `/unleash` → `/uf.unleash`, `/review-council` → `/uf.review-council`, `/constitution-check` → `/uf.constitution-check` references.

## 2. Namespace Migration — Blog Posts (#220)

Update command references in blog posts. Each task touches a different file.

- [x] 2.1 [P] Update `content/blog/unleash-in-practice.md`: Replace all `/unleash` → `/uf.unleash`, `/finale` → `/uf.finale` references. This file has the highest density of command references (~40+). Keep narrative coherent — update command names only, do not rewrite prose.
- [x] 2.2 [P] Update `content/blog/dewey-vs-karpathy.md`: Replace `/unleash` → `/uf.unleash`.
- [x] 2.3 [P] Update `content/blog/five-principles-every-ai-agent-harness-discovers.md`: Replace `/unleash` → `/uf.unleash`.
- [x] 2.4 [P] Update `content/blog/sandbox-isolation.md`: Replace `/unleash` → `/uf.unleash`.
- [x] 2.5 [P] Update `content/blog/the-8-phase-pipeline.md`: Replace `/unleash` → `/uf.unleash`.

## 3. Namespace Migration — Templates, Styles, and Internal Docs (#220)

- [x] 3.1 [P] Update `layouts/home.html`: Replace `/unleash` → `/uf.unleash` in the hero section code reference.
- [x] 3.2 [P] Update `assets/scss/common/_custom.scss`: Replace `/unleash` → `/uf.unleash` in the featured card comment.
- [x] 3.3 [P] Update `AGENTS.md`: Replace `/review-council` → `/uf.review-council` in the Review Council as PR Prerequisite section. Also update any other stale command references.

## 4. New Capability Documentation — Review Commands (#221, #224, #179)

These tasks modify `common-workflows.md` and `code-review-tutorial.md` (already touched in Group 1). They MUST run sequentially after Group 1.

- [x] 4.1 Update the `/uf.review-council` section in `content/docs/getting-started/common-workflows.md`: Add documentation for soft-gate CI causality analysis — pre-existing CI failures on `main` are non-blocking informational findings. Describe the two-tier baseline strategy (GitHub CI API check first, git worktree fallback). Document GitHub review posting capability — PR detection, verdict mapping (APPROVE/REQUEST_CHANGES/COMMENT), human confirmation requirement.
- [x] 4.2 Update the `/uf.review-pr` section in `content/docs/getting-started/common-workflows.md`: Document that verdict posting now occurs for all review outcomes regardless of finding severity (previously skipped for MEDIUM/LOW-only or zero-finding reviews).
- [x] 4.3 Update `content/docs/getting-started/code-review-tutorial.md`: Add coverage of soft-gate CI behavior and GitHub review posting to the relevant tutorial sections.

## 5. New Capability Documentation — Finale Command (#162, #204)

These tasks modify `common-workflows.md` (already touched in Groups 1 and 4). They MUST run sequentially after Group 4.

- [x] 5.1 Update the `/uf.finale` section in `content/docs/getting-started/common-workflows.md`: Document structured PR body format (Summary, How to Test, How to Demo, Key Files Changed), AI attribution footer and git trailer (`AI-assisted-by: /uf.finale`), and PR template detection.
- [x] 5.2 Update the `/uf.finale` section in `content/docs/getting-started/common-workflows.md`: Document the new sub-agent merge conflict resolution option (Option 5 in conflict recovery) — spawning a sub-agent to merge the target branch, identify conflicts, and resolve them with AI assistance.

## 6. New Command Documentation — Triage Issue (#202)

- [x] 6.1 Add a new section to `content/docs/getting-started/common-workflows.md` for `/uf.triage-issue`: Describe the multi-agent issue triage using 5 Divisor agents, 7 classification categories (bug, feature, enhancement, question, opinion, duplicate, needs-info), structured triage output with severity/priority/labels, and human-gated label mutations via AskUserQuestion.

## 7. Verification and Build

- [x] 7.1 Run `grep -rn` across `content/`, `layouts/`, `AGENTS.md` for any remaining old command names (`/unleash`, `/finale`, `/cobalt-crush`, `/constitution-check`, `/review-council`, `/uf-init`, `/review-pr`) that were missed. Fix any stragglers. Exclude `openspec/` and `specs/` directories (historical records).
- [x] 7.2 Run `npm run build` and verify no build errors. Check for broken internal links caused by anchor changes.
- [x] 7.3 Verify constitution alignment: confirm documentation changes are content-only with no agent behavior, artifact format, composability, or testability implications (all N/A per proposal).
<!-- spec-review: passed -->
<!-- code-review: passed -->
