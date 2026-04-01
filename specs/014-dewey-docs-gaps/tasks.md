# Tasks: Dewey Documentation Gaps

**Input**: Design documents from `/specs/014-dewey-docs-gaps/`
**Tests**: Not applicable. Validation is `npm run build` + visual verification.

## Format: `[ID] [P?] [Story] Description`

---

## Phase 1: User Story 1 — `uf init` Auto-Generated Config (Priority: P1)

**Goal**: Document what `uf init` auto-generates for Dewey sources before the individual source type reference sections.

**Independent Test**: Read knowledge.md and verify the auto-detection explanation, example config, and ownership model.

- [x] T001 [US1] Add a "What `uf init` Creates" subsection to content/docs/getting-started/knowledge.md before the "Configure Content Sources" section. Explain: `uf init` runs `dewey init` (bare default), then overwrites `sources.yaml` with an auto-detected multi-repo config. Cover sibling repo detection (scans `../` for `.git/` directories), GitHub org extraction (from `git remote get-url origin`), and graceful degradation (if no siblings or no GitHub remote, config has fewer sources). Source: research.md R1. (FR-001, FR-004)
- [x] T002 [US1] Add the example auto-generated `sources.yaml` to the "What `uf init` Creates" subsection in content/docs/getting-started/knowledge.md. Show per-repo disk sources for siblings, org-level disk source, and GitHub API source with auto-detected repos. Include YAML comments explaining each source group. Source: research.md R1. (FR-002)
- [x] T003 [US1] Add the ownership model explanation to the "What `uf init` Creates" subsection in content/docs/getting-started/knowledge.md. Explain: `sources.yaml` is user-owned after creation; `uf init` detects customization by counting `- id:` entries; if more than 1, it skips regeneration. Once customized, `uf init` never touches it again. Source: research.md R1. (FR-003)

**Checkpoint**: `npm run build` passes. Auto-detection behavior is documented.

---

## Phase 2: User Story 2 — Toolstack Web Source Guidance (Priority: P1)

**Goal**: Add proactive guidance for adding web sources for the project's toolstack, with Go and TypeScript templates.

**Independent Test**: Read knowledge.md and verify the "Extending Your Sources" section with benefit explanation and 2 language templates.

- [x] T004 [US2] Add an "Extending Your Sources" section to content/docs/getting-started/knowledge.md after the "Updating Sources" subsection (after the three source type references). Open with the benefit statement: agents reference current API documentation instead of training data, reducing hallucinated API calls and outdated patterns. Explain that `uf init` gives you disk and GitHub sources automatically, but web sources must be added manually because they are project-specific. Source: research.md R2. (FR-005, FR-006)
- [x] T005 [US2] Add a Go project web source template to the "Extending Your Sources" section in content/docs/getting-started/knowledge.md. Include entries for `pkg.go.dev/std`, `cobra.dev`, and `gorm.io/docs` with appropriate depth and refresh_interval settings. Source: research.md R2. (FR-007)
- [x] T006 [US2] Add a TypeScript/JavaScript project web source template to the "Extending Your Sources" section in content/docs/getting-started/knowledge.md. Include entries for the TypeScript handbook, React docs, and Next.js docs with appropriate depth and refresh_interval settings. Source: research.md R2. (FR-007)
- [x] T007 [US2] Add URL selection guidance to the "Extending Your Sources" section in content/docs/getting-started/knowledge.md. Cover: prefer stable documentation roots (not specific page URLs), use depth to control crawl scope, use refresh_interval for update frequency (weekly is a good default), and check dewey doctor for fetch errors. Source: research.md R2. (FR-008)

**Checkpoint**: `npm run build` passes. Web source guidance is complete with 2 templates.

---

## Phase 3: User Story 3 — Blog Post (Priority: P2)

**Goal**: Create a blog post about the stale-training-data problem and how Dewey web sources solve it.

**Independent Test**: Navigate to `/blog/` and verify the post exists with problem statement, before/after, and Getting Started.

- [x] T008 [US3] Create content/blog/dewey-knowledge-retrieval.md with frontmatter (title: "Your AI Agent Doesn't Read the Docs", description communicating the stale-training-data problem, lead, slug: "dewey-knowledge-retrieval", date, categories: Engineering, tags: dewey/knowledge-retrieval/semantic-search/documentation, contributors: Unbound Force) and write the full blog post covering: the problem (agents use stale training data), why it happens (training snapshots), the fix (index current toolstack docs), how Dewey does it (three source types, web crawl, semantic search), a concrete before/after example, and a Getting Started section linking to Quick Start and knowledge.md source configuration. Source: research.md R3. (FR-009 through FR-013)

**Checkpoint**: `npm run build` passes. Blog post renders on `/blog/`.

---

## Phase 4: User Story 4 — Cross-Reference (Priority: P3)

**Goal**: Add a cross-reference from the developer guide's `uf init` section to the source configuration guidance.

**Independent Test**: Read developer.md uf init section and verify it mentions sources.yaml and links to knowledge.md.

- [x] T009 [US4] Add a 1-2 sentence note to the Sub-Tool Initialization subsection of the "Project Scaffolding with `uf init`" section in content/docs/getting-started/developer.md. After the existing mention of `.dewey/` workspace creation, add: `uf init` auto-detects sibling repos and your GitHub org to generate a multi-repo sources config. Link to the knowledge page source configuration section for customization guidance (adding web sources for toolstack docs). (FR-014)

**Checkpoint**: `npm run build` passes. Cross-reference links correctly.

---

## Phase 5: Verification

- [x] T010 Run `npm run build` and verify zero errors (FR-015)
- [x] T011 Verify all cross-links resolve by clicking each link in the dev server (`npm run dev`): knowledge.md internal anchors, blog post URL at `/blog/dewey-knowledge-retrieval/`, developer.md link to knowledge.md source configuration section (FR-016)

<!-- spec-review: passed -->
