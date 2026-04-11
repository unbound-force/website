# Tasks: How Dewey Became a Knowledge Curator Blog Post

**Input**: Design documents from `/specs/021-dewey-curator-blog/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, quickstart.md

**Tests**: No automated tests. Validation is manual: `npm run build` (zero errors) + visual verification in `npm run dev`.

**Organization**: Tasks are grouped by user story. All tasks modify a single file (`content/blog/dewey-curator-blog.md`), so there are no parallel opportunities — tasks are strictly sequential.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

---

## Phase 1: Setup

**Purpose**: Verify the build environment is healthy before creating new content.

- [x] T001 Run `npm run build` and confirm zero errors — baseline build health before changes

**Checkpoint**: Build passes. No pre-existing issues that could be confused with regressions from the new post.

---

## Phase 2: User Story 1 — Narrative Journey from Comparison to Implementation (Priority: P1) 🎯 MVP

**Goal**: Write the blog post with the five-section narrative structure from issue #30. A reader who followed the Karpathy comparison post recognizes this as a natural sequel. A reader without prior context gets a self-contained engineering story.

**Independent Test**: The post reads as a coherent narrative from problem → solution → architectural insight → lessons learned. The predecessor post link works. No CLI syntax is duplicated from docs.

### Implementation for User Story 1

- [x] T002 [US1] Create `content/blog/dewey-curator-blog.md` with frontmatter: title, description, lead, slug (`dewey-curator`), date (`2026-04-11T00:00:00+00:00`), draft (`false`), weight (`58`), toc (`true`), categories (`["Engineering"]`), tags (`["dewey", "knowledge-management", "knowledge-compilation", "karpathy", "event-sourcing"]`), contributors (`["Unbound Force"]`). Match format from `content/blog/dewey-vs-karpathy.md`.
- [x] T003 [US1] Write Section 1 — "The Problem: Learnings That Never Compound" in `content/blog/dewey-curator-blog.md`. Describe the problem of AI agent learnings that accumulate but never synthesize. Reference the predecessor post ("The Librarian vs The Index") with a link to `/blog/dewey-vs-karpathy/`. Establish the narrative thread: that post speculated about `dewey compile` and contamination separation — this post tells the story of building them. Per FR-003, FR-012.
- [x] T004 [US1] Write Section 2 — "From Karpathy to Code: Three Key Ideas" in `content/blog/dewey-curator-blog.md`. Describe the three ideas from Karpathy's approach that inspired Dewey v3.0.0: (1) autonomous synthesis (compile), (2) contamination separation (trust tiers), (3) self-healing linting. Credit Karpathy fairly per FR-011 — frame as "inspired by" not "better than." Acknowledge where Karpathy's approach excels (small curated datasets, personal research). Per FR-011, FR-012.
- [x] T005 [US1] Write Section 3 — "The Event Sourcing Insight" in `content/blog/dewey-curator-blog.md`. Explain the event sourcing architecture: raw learnings as an append-only event log, compiled articles as materialized views rebuilt from learnings. Include category-aware resolution (decisions=temporal merge, patterns=accumulate, gotchas=dedup) as a concrete example. Explain the dual-mode LLM pattern (agent LLM for MCP, local Ollama for CLI). Per FR-004, FR-007, research items R5, R6.
- [x] T006 [US1] Write Section 4 — "Knowledge Linting: The Self-Healing Index" in `content/blog/dewey-curator-blog.md`. Explain the four quality checks (stale decisions, uncompiled learnings, embedding gaps, contradictions). Distinguish mechanical fixes (auto-fixable with `--fix`) from semantic fixes (require compilation or human judgment). Explain trust tiers (authored/validated/draft) and how `dewey promote` moves content between tiers. Per FR-005, FR-006, research items R7.
- [x] T007 [US1] Write Section 5 — "What We Learned Building It" in `content/blog/dewey-curator-blog.md`. Include at least two genuine engineering insights per FR-013: (1) category-aware resolution as a prompt engineering insight, (2) the leaf dependency pattern for the `llm/` package (zero Dewey imports). Cite the 124 tests / 7 packages metric. Do not overclaim maturity. End the section with a brief call-to-action paragraph directing readers to try the features — link to the [knowledge getting-started guide](/docs/getting-started/knowledge/) and the [Dewey project page](/docs/projects/dewey/). Per FR-013, BA-001 (narrative arc with conclusion/CTA), research item R8.

**Checkpoint**: The five-section blog post is complete. The narrative flows from problem → inspiration → architecture → quality → lessons. All content is sourced from research.md (PR #42, spec 013, predecessor post). No CLI syntax is inlined.

---

## Phase 3: User Story 2 — Architectural Insights for Technical Readers (Priority: P1)

**Goal**: Review and polish the post to ensure architectural insights (event sourcing, dual-mode LLM) are clearly communicated and independently valuable. A technical reader can identify and articulate at least two patterns they could apply to their own projects.

**Independent Test**: A technical reader can identify the event sourcing pattern and the dual-mode LLM pattern from the post without needing external context.

### Implementation for User Story 2

- [x] T008 [US2] Review Section 3 (Event Sourcing) and Section 4 (Knowledge Linting) in `content/blog/dewey-curator-blog.md` for architectural clarity. Verify: (1) the event sourcing analogy is explicit — "raw learnings are events, compiled articles are materialized views," (2) the dual-mode LLM pattern is clearly explained — why agent LLM for MCP and local Ollama for CLI, (3) the mechanical vs semantic fix distinction is clear. Refine prose as needed. Per US2 acceptance scenarios.
- [x] T009 [US2] Verify the four knowledge linting checks are each named and explained in `content/blog/dewey-curator-blog.md`: stale decisions (>30 days), uncompiled learnings, embedding gaps, contradictions. Verify the mechanical/semantic distinction is clear. Per US2 acceptance scenario 3.

**Checkpoint**: Architectural insights are clearly communicated. A technical reader can identify event sourcing and dual-mode LLM as reusable patterns.

---

## Phase 4: User Story 3 — Links to Updated Documentation (Priority: P2)

**Goal**: Verify all internal links resolve to valid pages and point to updated docs (spec 020). CLI syntax is linked, not duplicated.

**Independent Test**: Every internal link in the post resolves. No fenced code blocks contain CLI usage syntax.

### Implementation for User Story 3

- [x] T010 [US3] Verify all internal links in `content/blog/dewey-curator-blog.md` resolve to valid pages. Required links per quickstart.md V4: `/blog/dewey-vs-karpathy/` (predecessor post), `/docs/getting-started/knowledge/` (knowledge guide), `/docs/projects/dewey/` (Dewey project page). Fix any broken or missing links. Per FR-008, US3 acceptance scenarios.
- [x] T011 [US3] Verify no fenced code blocks in `content/blog/dewey-curator-blog.md` contain `dewey compile`, `dewey lint`, or `dewey promote` CLI usage syntax. Command names may appear in prose but usage examples must link to docs. Per FR-009, quickstart.md V8.

**Checkpoint**: All internal links resolve. No duplicated CLI syntax. Readers can follow links to authoritative reference docs.

---

## Phase 5: Polish & Verification

**Purpose**: Final quality checks across all user stories before merge.

- [x] T012 Verify word count of `content/blog/dewey-curator-blog.md` body text (excluding frontmatter) is 1200–1800 words. Adjust if outside range. Per spec constraint, quickstart.md V6.
- [x] T013 Verify no time-sensitive language in `content/blog/dewey-curator-blog.md`: no "just shipped," "this week," "recently," "now available," or similar phrases. The frontmatter `date` field provides temporal context. Per FR-010, quickstart.md V7.
- [x] T014 Run `npm run build` and confirm zero errors with the new blog post included. Per FR-014, quickstart.md V1.
- [x] T015 Run `npm run dev` and visually verify `content/blog/dewey-curator-blog.md` renders correctly: title, lead text, table of contents, five sections with H2 headings, internal links clickable. Verify both light and dark mode. Per FR-015, quickstart.md V2, V3.

**Checkpoint**: Post is publication-ready. Build passes, visual rendering is correct, all quality checks pass.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — confirms build health
- **US1 (Phase 2)**: Depends on Phase 1 — creates the blog post file and writes all content
- **US2 (Phase 3)**: Depends on Phase 2 — reviews and polishes architectural sections
- **US3 (Phase 4)**: Depends on Phase 2 — verifies links and no duplicated CLI syntax
- **Polish (Phase 5)**: Depends on Phases 2, 3, 4 — final quality gate

### Parallel Opportunities

**None.** All tasks modify or review a single file (`content/blog/dewey-curator-blog.md`). Tasks must be executed sequentially.

### Execution Strategy

Sequential, single-file workflow:

1. Phase 1: Verify build health (T001)
2. Phase 2: Write the complete blog post (T002–T007)
3. Phase 3: Polish architectural clarity (T008–T009)
4. Phase 4: Verify links and no CLI duplication (T010–T011)
5. Phase 5: Final quality checks (T012–T015)

---

## Notes

- All content is sourced from research.md (PR #42, spec 013, predecessor post). No fabrication.
- The post is a narrative sequel to `content/blog/dewey-vs-karpathy.md` — maintain the same balanced, fair tone toward Karpathy's approach.
- CLI syntax lives in docs (updated by spec 020). The blog post describes concepts and architecture only.
- Commit after each phase checkpoint, not after individual tasks.

<!-- spec-review: passed -->
