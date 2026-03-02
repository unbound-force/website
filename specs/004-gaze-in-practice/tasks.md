# Tasks: "Gaze in Practice" Blog Post and Docs Sample Output

**Input**: Design documents from `/specs/004-gaze-in-practice/`
**Prerequisites**: plan.md (required), spec.md (required for user stories)

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2)
- Exact file paths included in descriptions

---

## Phase 1: User Story 1 — Blog Post (Priority: P1) 🎯 MVP

**Goal**: Publish a blog post walking through real Gaze output section by section with interpretive narrative.

**Independent Test**: Navigate to `/blog/gaze-in-practice/`. The full article renders with all tables, correct data, and cross-links.

### Implementation

- [x] T001 [US1] Create blog post file at `content/blog/gaze-in-practice.md` with YAML frontmatter (title: "Gaze in Practice: A Real-World Quality Report Walkthrough", slug: `gaze-in-practice`, date: 2026-03-02, weight: 90, categories: `["Engineering"]`, tags: `["gaze", "testing", "go", "quality"]`, contributors: `["Unbound Force"]`, toc: true, draft: false)
- [x] T002 [US1] Write introduction section: establish context (gcal-organizer project, branch 008-decision-extraction, Gaze v1.2.3), explain why a real project matters vs synthetic examples, link to "Why Contract Coverage" blog post for conceptual background
- [x] T003 [US1] Write CRAP Summary section: reproduce the CRAP Summary table from `temp/tmp.md` (137 functions, avg complexity 4.9, avg coverage 26.2%, avg CRAP 29.7, CRAPload 40), followed by interpretation of what these numbers reveal about the project
- [x] T004 [US1] Write Top 5 Worst CRAP Scores section: reproduce the Top 5 table (CreateDecisionsTab 650, runBrowserScript 342, loadDotEnv 240, ListMeetingDocuments 210, printSummary 156), followed by interpretation highlighting that all five have 0% coverage
- [x] T005 [US1] Write GazeCRAP Quadrant Distribution section: reproduce the quadrant table (Q1: 12, Q2: 0, Q3: 3, Q4: 2), reproduce the GazeCRAPload narrative (5 functions, SyncCalendarAttachments at 1482, OrganizeDocuments at 306), followed by interpretation of what the quadrant distribution tells us
- [x] T006 [US1] Write Quality Summary section: reproduce the warning about 0 tests at module level, interpret honestly — explain what this means (per-package analysis needed) and why it appears
- [x] T007 [US1] Write Classification Summary section: reproduce the warning about only 1 function classified and no documentation found, interpret honestly — explain mechanical vs document-enhanced classification
- [x] T008 [US1] Write Overall Health Assessment section: reproduce the Summary Scorecard table (CRAPload D, GazeCRAPload A-, Avg Line Coverage D, Contract Coverage C, Complexity B+), reproduce the Top 5 Prioritized Recommendations, followed by interpretation
- [x] T009 [US1] Write wrap-up section: summarize key takeaways, explain how a developer would prioritize next steps using this report, link to Gaze project docs page for setup instructions

**Checkpoint**: Blog post is complete and ready for build verification.

---

## Phase 2: User Story 2 — Docs Page Sample Output (Priority: P2)

**Goal**: Add a condensed sample output section to the Gaze docs page.

**Independent Test**: Navigate to `/docs/projects/gaze/`. The new "Sample Output" section appears between "Three Modes" and "Architecture" with correct tables and a link to the blog post.

### Implementation

- [x] T010 [US2] Add new `## Sample Output` section to `content/docs/projects/gaze.md` after the "Three Modes" subsection (after the `/classify-docs` paragraph, before `## Architecture`). Include: one-sentence intro, project identification (gcal-organizer, Gaze v1.2.3), Health Assessment scorecard table, Top 5 Worst CRAP Scores table, link to blog post ("For a section-by-section walkthrough, see [Gaze in Practice](/blog/gaze-in-practice/).")

**Checkpoint**: Docs page update is complete and ready for build verification.

---

## Phase 3: Verification

**Purpose**: Confirm all content renders correctly and the site builds without errors.

- [x] T011 Run `npm run build` and confirm zero errors
- [x] T012 Run `npm run dev` and visually verify: (a) blog index lists both posts with "Gaze in Practice" appearing first, (b) blog post renders all tables and code blocks correctly, (c) homepage "Latest Articles" shows "Gaze in Practice", (d) Gaze docs page shows new section with correct tables, (e) all cross-links work (blog ↔ docs, blog → "Why Contract Coverage"), (f) light and dark mode render correctly
- [x] T013 Verify data accuracy: cross-reference all numbers in blog post and docs section against `temp/tmp.md` source output

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (Blog Post)**: No dependencies — can start immediately
- **Phase 2 (Docs Update)**: No dependency on Phase 1 (different file), but logically should be done after so the blog link target exists
- **Phase 3 (Verification)**: Depends on Phase 1 and Phase 2 completion

### Parallel Opportunities

- T001 through T009 are sequential (single file, each section builds on the previous)
- T010 is independent of T001-T009 (different file) but should logically follow to ensure blog post exists for cross-linking
- T011, T012, T013 run after all content tasks are complete

### Implementation Strategy

Sequential delivery in priority order:
1. Complete Phase 1 (blog post) — the primary deliverable
2. Complete Phase 2 (docs update) — quick addition referencing the blog post
3. Complete Phase 3 (verification) — confirm everything works
