# Tasks: Unleash Visibility

**Input**: Design documents from `/specs/013-unleash-visibility/`
**Tests**: Not applicable. Static documentation site. Validation is `npm run build` + visual verification.

## Format: `[ID] [P?] [Story] Description`

---

## Phase 1: User Story 1 — Blog Post (Priority: P1)

**Goal**: Create a blog post that walks through the `/unleash` pipeline from spec to demo.

**Independent Test**: Navigate to `/blog/` and verify the post exists with all 8 pipeline steps, exit points, and a Getting Started section.

- [x] T001 [US1] Create content/blog/unleash-in-practice.md with frontmatter (title, description, lead, slug, date, categories: Engineering, tags: unleash/swarm/autonomous/pipeline, contributors: Unbound Force) and write the full blog post covering: the problem (manual multi-step pipelines), what /unleash does (single command, 8 steps), the 8-step pipeline walkthrough, exit points and resumability, key capabilities (Dewey clarification, Swarm parallelism, Divisor review, Hivemind learning), the unleash+finale loop, and a Getting Started section linking to Quick Start. Source: research.md R1, R6. (FR-001 through FR-006, FR-018)

**Checkpoint**: `npm run build` must succeed. Blog post renders on `/blog/`.

---

## Phase 2: User Story 2 — Entry-Point Cross-References (Priority: P1)

**Goal**: Surface `/unleash` on 5 entry-point pages so visitors discover it regardless of entry path.

**Independent Test**: Search each of the 5 pages for "unleash" and verify it appears with a link.

- [x] T002 [P] [US2] Update the "Speckit Workflow" feature card in layouts/home.html to communicate the one-command autonomous pipeline. Change heading to "One-Command Pipeline" and rewrite description to lead with /unleash. (FR-007, FR-016)
- [x] T003 [P] [US2] Update the "Start Working" section in content/docs/getting-started/quick-start.md to present /unleash as the first and simplest option, followed by /speckit.specify for manual control. (FR-008)
- [x] T004 [P] [US2] Add a prominent /unleash callout note before the individual Speckit command listing in the "Working with Speckit" section of content/docs/getting-started/developer.md. (FR-009)
- [x] T005 [P] [US2] Update the Common Workflows bullet description in content/docs/getting-started/\_index.md to surface the autonomous pipeline capability. (FR-010)
- [x] T006 [P] [US2] Add /unleash as the named command alongside /workflow seed in the autonomous workflow section of content/docs/getting-started/product-owner.md. (FR-011)

**Checkpoint**: `npm run build` must succeed. All 5 pages mention `/unleash`.

---

## Phase 3: User Story 3 — Restructure Common Workflows (Priority: P2)

**Goal**: Move the Autonomous Pipeline section to the top of common-workflows.md so it's the first workflow a reader encounters.

**Independent Test**: Open common-workflows.md and verify the Autonomous Pipeline section appears before the manual New Feature section.

- [x] T007 [US3] Reorder the H2 sections in content/docs/getting-started/common-workflows.md: move Autonomous Pipeline (/unleash) to position 1, End-of-Branch Workflow (/finale) to position 2, keep New Feature (End-to-End) at position 3. Use Hugo's explicit heading ID syntax `{#new-feature-end-to-end}` on the New Feature heading to guarantee anchor stability regardless of future heading text edits. (FR-012, FR-013, FR-015)
- [x] T008 [US3] Add a note at the top of the New Feature (End-to-End) section in content/docs/getting-started/common-workflows.md indicating it is the step-by-step alternative to /unleash for when fine-grained control is needed. (FR-014)

**Checkpoint**: `npm run build` must succeed. TOC shows new order. `#new-feature-end-to-end` anchor preserved.

---

## Phase 4: Verification

- [x] T009 Run `npm run build` and verify zero errors (FR-019)
- [x] T010 Count pages mentioning /unleash -- must be at least 7 (SC-001)
- [x] T011 Verify anchor #new-feature-end-to-end is preserved in common-workflows.md (SC-006)

<!-- spec-review: passed -->
