# Tasks: Dewey v3.0.0 Documentation Sync

**Input**: Design documents from `/specs/020-dewey-v3-docs-sync/`
**Prerequisites**: plan.md, spec.md, research.md, quickstart.md

**Tests**: No automated tests — this is a Hugo static site. Validation is `npm run build` (must succeed) and `npm run dev` (visual verification).

**Organization**: US1 (tool counts) and US2 (knowledge lifecycle) are both P1 and touch all three files, but individual tasks within each story target different files and can be parallelized. US3 (trust tiers) and US4 (store_learning API) are P2 and both target `knowledge.md` — they must be sequential. US5 (web crawler note) is P3 and targets `knowledge.md`.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

---

## Phase 1: Setup

**Purpose**: Verify the site builds cleanly and audit all current tool count / source type mentions before making changes.

- [x] T001 Run `npm run build` to verify the site builds cleanly before any changes
- [x] T002 Search the entire `content/` directory for all occurrences of "40 MCP", "40 tools", "10 categories", "three.*source", and "Three.*Source" to identify every line that needs updating. Record line numbers for each file. Cross-reference against plan.md Change Detail section to ensure no mentions are missed.

**Checkpoint**: `npm run build` passes. All update targets identified.

---

## Phase 2: User Story 1 — Accurate Tool Count and Capabilities (Priority: P1) 🎯 MVP

**Goal**: Every page that mentions Dewey's MCP tool count, category count, or source type count states the correct v3.0.0 numbers (48 tools, 12 categories, 4 source types). All three pages agree.

**Independent Test**: Search the built site for all mentions of "MCP tools", "categories", and "source types" — every instance states the same correct numbers.

### Project Page Updates

- [x] T003 [P] [US1] In `content/docs/projects/dewey.md` line 17: update "40 MCP tools across 10 categories" → "48 MCP tools across 12 categories"
- [x] T004 [P] [US1] In `content/docs/projects/dewey.md` line 38: update heading "### 40 MCP Tools Across 10 Categories" → "### 48 MCP Tools Across 12 Categories"
- [x] T005 [P] [US1] In `content/docs/projects/dewey.md` line 40: update body text to state "48 MCP tools across 12 categories" and list the 12 categories: navigate, search, analyze, write, decision, journal, flashcard, whiteboard, semantic search, health, knowledge management, and learning
- [x] T006 [P] [US1] In `content/docs/projects/dewey.md` line 46: update heading "### Three Content Source Types" → "### Four Content Source Types"
- [x] T007 [P] [US1] In `content/docs/projects/dewey.md` lines 48-50: add a fourth bullet after the web crawl bullet: "**Code** — Go AST parsing to extract function signatures, CLI commands, MCP tool registrations, and package documentation"
- [x] T008 [P] [US1] In `content/docs/projects/dewey.md` line 75: update "disk, GitHub, and web crawl" → "disk, GitHub, web crawl, and code" in the Architecture section's pluggable sources bullet

### Team Page Updates

- [x] T009 [P] [US1] In `content/docs/team/dewey.md` line 35: update "40 MCP tools across 10 categories" → "48 MCP tools across 12 categories"
- [x] T010 [P] [US1] In `content/docs/team/dewey.md` line 72: update "three pluggable source types" → "four pluggable source types"
- [x] T011 [P] [US1] In `content/docs/team/dewey.md`: add a "### Code" subsection after the "### Web Crawl" section (after line 84), describing Go AST parsing that extracts function signatures, CLI commands, MCP tool registrations, and package documentation from Go source files

### Knowledge Guide Updates

- [x] T012 [P] [US1] In `content/docs/getting-started/knowledge.md` line 147: update "three pluggable source types" → "four pluggable source types"
- [x] T013 [P] [US1] In `content/docs/getting-started/knowledge.md`: add a "### Code" source configuration section after the "### Web Crawl" section (before "### Updating Sources"), with a YAML example showing a code source configuration and a table of configuration fields. Describe Go AST parsing for function signatures, CLI commands, MCP tool registrations, and package documentation.

- [x] T014 [US1] Run `npm run build` to verify all tool count and source type updates build without errors

**Checkpoint**: All three pages state "48 MCP tools across 12 categories" and "four content source types" consistently. Build passes.

---

## Phase 3: User Story 2 — Knowledge Compilation, Linting, and Promotion Documentation (Priority: P1)

**Goal**: Visitors can find and understand compile, lint, and promote capabilities on the project page, knowledge guide, and team page.

**Independent Test**: Navigate from the Dewey project page to compile/lint/promote descriptions within two clicks. Verify all three pages document these features.

### Project Page — Knowledge Lifecycle Section

- [x] T015 [P] [US2] In `content/docs/projects/dewey.md`: add a "### Knowledge Lifecycle" section after the "### Graceful Degradation" section (after line 58), describing compile, lint, and promote at evaluator level — what each does, why it matters, and how they form a lifecycle (raw learnings → compiled articles → linted for quality → promoted through trust tiers). Keep it concise (3-4 paragraphs). Source content from research.md R3.

### Knowledge Guide — CLI Commands

- [x] T016 [P] [US2] In `content/docs/getting-started/knowledge.md`: add a "## Knowledge Lifecycle" section after the "## Graceful Degradation" section (before "## Troubleshooting"). Include subsections for `dewey compile`, `dewey lint`, and `dewey promote` CLI commands with usage descriptions, example invocations, and brief explanations of what each command does. Source content from research.md R3.

### Team Page — MCP Tools Table

- [x] T017 [P] [US2] In `content/docs/team/dewey.md`: add a "Knowledge Management" category to the MCP tools table in the "## Query Capabilities" section. Add rows for `compile` (cluster learnings by topic and synthesize into articles), `lint` (detect stale decisions, uncompiled learnings, embedding gaps, contradictions), and `promote` (move content between trust tiers). Place after the existing Semantic Queries table.

- [x] T018 [US2] Run `npm run build` to verify all knowledge lifecycle additions build without errors

**Checkpoint**: Compile, lint, and promote are documented on all three pages. Two-click discoverability from project page confirmed. Build passes.

---

## Phase 4: User Story 3 — Trust Tiers Documentation (Priority: P2)

**Goal**: Visitors can find and understand the three trust tiers (authored, validated, draft) and how they affect search and retrieval.

**Independent Test**: A visitor can find the three trust tiers on the knowledge guide and understand their meaning without reading source code.

- [x] T019 [US3] In `content/docs/getting-started/knowledge.md`: add a "## Trust Tiers" section after the new "## Knowledge Lifecycle" section. Include a table with three rows (authored, validated, draft) showing tier name, meaning, and how content gets that tier. Explain that trust tiers affect search result ranking and that `dewey_semantic_search_filtered` accepts a `tier` parameter for filtering. Source content from research.md R4.

- [x] T020 [US3] Run `npm run build` to verify trust tiers section builds without errors

**Checkpoint**: Trust tier documentation exists with all three tiers explained. Build passes.

---

## Phase 5: User Story 4 — Updated store_learning API Documentation (Priority: P2)

**Goal**: Visitors can find accurate `store_learning` tool documentation with current parameter names and response format.

**Independent Test**: A visitor can find `store_learning` documented with its `tag` (required), `category` (optional), and `information` (required) parameters, the `{tag}-{sequence}` response format, and the search result metadata fields.

- [x] T021 [US4] In `content/docs/getting-started/knowledge.md`: add a "## Storing and Retrieving Learnings" section after the "## Trust Tiers" section. Document the `store_learning` tool with a parameter table (`information`: required string; `tag`: required string, singular; `category`: optional enum with values decision/pattern/gotcha/context/reference). Document the response format (`{tag}-{sequence}` identity string, e.g., `gotcha-003`). Document that `semantic_search` results include `created_at`, `category`, and `tier` metadata fields. Source content from research.md R5.
- [x] T022 [US4] In `content/docs/team/dewey.md`: add a "Learning" category to the MCP tools table with a row for `store_learning` (store learnings with tag and optional category classification). This should be placed after the new "Knowledge Management" category added in T017.

- [x] T023 [US4] Run `npm run build` to verify store_learning API documentation builds without errors

**Checkpoint**: `store_learning` API is fully documented with current v3.0.0 parameters. Build passes.

---

## Phase 6: User Story 5 — Bug Fix Note for Web Crawler (Priority: P3)

**Goal**: Visitors reading about web crawling learn that complex documentation sites are now supported.

**Independent Test**: The web source section of the knowledge guide mentions complex documentation sites like pkg.go.dev.

- [x] T024 [US5] In `content/docs/getting-started/knowledge.md`: add a sentence to the "### Web Crawl" section (after line 224, after the description of robots.txt and caching) noting that complex documentation sites such as pkg.go.dev are fully supported by the web crawler.

- [x] T025 [US5] Run `npm run build` to verify the web crawler note builds without errors

**Checkpoint**: Web crawler reliability note added. Build passes.

---

## Phase 7: Polish & Cross-Cutting Concerns

**Purpose**: Final consistency verification, visual checks, and cross-page reconciliation.

- [x] T026 Search the entire `content/` directory for any remaining occurrences of "40 MCP", "40 tools", "10 categories", "three.\*source" (case-insensitive). Verify zero matches remain on the three affected pages. Note: the blog post `dewey-vs-karpathy.md` mentions "40 tools" — this is explicitly out of scope per the spec and should NOT be changed.
- [x] T027 Run `npm run build` to verify the complete site builds with zero errors
- [x] T028 Run `npm run dev` and visually verify all three updated pages render correctly in both light and dark mode. Check that tables have visible borders/separators, code blocks are readable, and no text-on-same-color-background issues exist.
- [x] T029 Verify the two-click discoverability path: homepage → Dewey project page → compile/lint/promote descriptions. Click each internal link on all three updated pages and confirm they resolve (no 404s).
- [x] T030 Cross-reference all factual claims on updated pages against research.md: verify tool count (48), category count (12), source type count (4), trust tier names (authored/validated/draft), `store_learning` parameters (`tag` singular required, `category` optional), and response format (`{tag}-{sequence}`). Confirm no capability is fabricated or overstated (FR-014).
- [x] T031 Verify no page contains placeholder text, "Coming Soon", or TODO markers (Constitution Principle I: no placeholder content)

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — can start immediately
- **US1 (Phase 2)**: Depends on Phase 1 — needs audit results from T002
- **US2 (Phase 3)**: Depends on Phase 2 completion — knowledge lifecycle sections reference tool counts established in Phase 2
- **US3 (Phase 4)**: Depends on Phase 3 — trust tiers section is placed after knowledge lifecycle section in `knowledge.md`
- **US4 (Phase 5)**: Depends on Phase 4 — store_learning section is placed after trust tiers section in `knowledge.md`; team page MCP table additions depend on T017
- **US5 (Phase 6)**: Can run in parallel with Phases 3-5 (different section of `knowledge.md`, no content dependency)
- **Polish (Phase 7)**: Depends on all previous phases

### Within-Phase Parallel Opportunities

- **Phase 2 (US1)**: T003-T008 (project page), T009-T011 (team page), and T012-T013 (knowledge guide) can run in parallel — different files
- **Phase 3 (US2)**: T015 (project page), T016 (knowledge guide), and T017 (team page) can run in parallel — different files
- **Phase 5 (US4)**: T021 (knowledge guide) and T022 (team page) can run in parallel — different files

### Cross-Phase Sequencing for `knowledge.md`

The knowledge guide receives the most additions. New sections must be added in this order to maintain correct document flow:

1. **T013** (US1): "### Code" source config section — after Web Crawl, before Updating Sources
2. **T024** (US5): Web crawler reliability note — within existing Web Crawl section
3. **T016** (US2): "## Knowledge Lifecycle" section — after Graceful Degradation, before Troubleshooting
4. **T019** (US3): "## Trust Tiers" section — after Knowledge Lifecycle
5. **T021** (US4): "## Storing and Retrieving Learnings" section — after Trust Tiers

---

## Notes

- This is a **documentation sync** — all tasks are Markdown file edits, no code changes
- Content is sourced from upstream Dewey PR #42 (spec 013-knowledge-compile), research.md, and the Dewey repository
- The blog post (`dewey-vs-karpathy.md`) is explicitly **out of scope** — do not modify it
- Use `##` headings (H2) for body content — the `title` frontmatter generates H1
- All new content must follow Doks/Hugo conventions: ATX-style headings, fenced code blocks with language identifiers
- Adapt content for a website audience — less raw technical detail, more "why should I care" framing (per AGENTS.md Content Sources guidance)
- The plan.md Cross-Page Consistency Requirements table is the authoritative reference for values that must match across pages

<!-- spec-review: passed -->
