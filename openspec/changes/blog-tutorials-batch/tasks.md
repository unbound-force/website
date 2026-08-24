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

## 1. Tutorials Section Infrastructure

- [x] 1.1 Create `content/docs/tutorials/_index.md` with section frontmatter (title: "Tutorials", description, lead, date, draft: false, weight: 100, toc: false)
- [x] 1.2 Add tutorials navigation entry to `config/_default/menus/menus.en.toml` at weight 37 with identifier "tutorials" and url "/docs/tutorials/"

## 2. Blog Posts

All blog posts are independent files. Each follows the established frontmatter pattern from existing posts (title, description, lead, slug, date: 2026-08-23, draft: false, weight, toc: true, categories, tags, contributors). Content is sourced from the corresponding GitHub issue outline.

- [x] 2.1 [P] Create `content/blog/ci-failure-classification.md` — How /review-pr separates signal from noise (issue #64, weight: 101)
- [x] 2.2 [P] Create `content/blog/agents-md-quality.md` — Why AGENTS.md quality determines code quality (issue #65, weight: 102)
- [x] 2.3 [P] Create `content/blog/dewey-knowledge-stores.md` — Dewey v3.1.0 knowledge stores (issue #69, weight: 103)
- [x] 2.4 [P] Create `content/blog/dewey-slash-command-ux.md` — Zero-code UX with /dewey-store (issue #72, weight: 104)
- [x] 2.5 [P] Create `content/blog/pluggable-llm-providers.md` — Dewey cloud-optional providers (issue #116, weight: 105)
- [x] 2.6 [P] Create `content/blog/gaze-baseline-comparison.md` — Per-function CRAP regression detection (issue #150, weight: 106)
- [x] 2.7 [P] Create `content/blog/council-review-action.md` — Divisor Council in GitHub Actions (issue #191, weight: 107)
- [x] 2.8 [P] Create `content/blog/prompt-hardening.md` — Prompt compression resilience (issue #198, weight: 108)
- [x] 2.9 [P] Create `content/blog/multi-platform-support.md` — uf init for OpenCode and Cursor (issue #51, weight: 109)

## 3. Tutorials

All tutorials are independent files under the tutorials section created in group 1. Each follows docs frontmatter conventions (title, description, lead, date: 2026-08-23, draft: false, weight, toc: true). Content is sourced from the corresponding GitHub issue outline.

- [x] 3.1 [P] Create `content/docs/tutorials/dewey-knowledge-stores.md` — Setting up knowledge stores (issue #70, weight: 10)
- [x] 3.2 [P] Create `content/docs/tutorials/dewey-provider-configuration.md` — Configuring embedding/synthesis providers (issue #114, weight: 20)
- [x] 3.3 [P] Create `content/docs/tutorials/gaze-crap-baseline-ci.md` — CRAP baseline comparison in CI (issue #151, weight: 30)
- [x] ~~3.4 [P] Create `content/docs/tutorials/pinkman-oss-scout.md` — Getting started with /scout (issue #45, weight: 40)~~ **Removed per user direction**
- [x] 3.5 [P] Create `content/docs/tutorials/onboard-customization.md` — Getting started with /onboard (issue #46, weight: 50)

## 4. Validation

- [x] 4.1 Run `npm run build` and verify all 15 new pages appear in the `public/` output with no build errors
- [x] 4.2 Verify tutorials section appears in sidebar at correct position (between Reference and Changelog) and blog posts appear in blog listing via `npm run dev` visual check

<!-- spec-review: passed -->
<!-- code-review: passed -->
