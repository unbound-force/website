# Tasks: Dewey Documentation

**Input**: Design documents from `/specs/007-dewey-docs/`
**Prerequisites**: plan.md, spec.md, research.md, quickstart.md

**Tests**: No automated tests -- this is a Hugo static site. Validation is `npm run build` (must succeed) and `npm run dev` (visual verification).

**Organization**: US1 and US2 are both P1 but can be written in parallel (different files). US3 and US4 are P2 and can also be parallelized.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to
- Include exact file paths in descriptions

## Phase 1: Setup

**Purpose**: Verify the project builds and understand the existing page structure before adding content.

- [x] T001 Run `npm run build` to verify the site builds cleanly before any changes
- [x] T002 Read the existing getting-started pages (`content/docs/getting-started/*.md`) and team pages (`content/docs/team/*.md`) to understand the current frontmatter conventions, heading styles, and content tone

---

## Phase 2: User Story 1 - Dewey Getting Started Guide (Priority: P1)

**Goal**: A new developer can find, read, and follow the Dewey getting-started guide to install, configure, and verify Dewey.

**Independent Test**: Navigate to `/docs/getting-started/knowledge/` in the dev server. Verify the page covers installation, initialization, source configuration, indexing, verification, 3-tier degradation, and OpenCode integration. Verify it appears in the sidebar.

- [x] T003 [US1] Create `content/docs/getting-started/knowledge.md` with frontmatter: title "Knowledge Retrieval with Dewey", description, lead, date, weight 80, toc true, draft false
- [x] T004 [US1] Write the "What is Dewey?" section in `content/docs/getting-started/knowledge.md`: 1-2 paragraphs explaining Dewey as a semantic knowledge layer that gives AI agents cross-repo context. Mention it replaces graphthulhu. Frame around "why should I care" per Constitution Principle III
- [x] T005 [US1] Write the "Installation" section in `content/docs/getting-started/knowledge.md`: Homebrew install command (`brew install unbound-force/tap/dewey`), Ollama + embedding model pull, and `go install` fallback with note that Homebrew may not yet be available
- [x] T006 [US1] Write the "Getting Started" section in `content/docs/getting-started/knowledge.md`: `dewey init`, editing `.dewey/config.yaml`, `dewey index`, `dewey status` walkthrough
- [x] T007 [US1] Write the "Source Configuration" section in `content/docs/getting-started/knowledge.md`: YAML examples for all 3 source types (local disk, GitHub API with whitelisted repos, web crawl with depth and refresh). Use the examples from the design paper Section 4
- [x] T008 [US1] Write the "OpenCode Integration" section in `content/docs/getting-started/knowledge.md`: the `opencode.json` MCP config entry for Dewey, note that `uf init` generates this automatically
- [x] T009 [US1] Write the "Graceful Degradation" section in `content/docs/getting-started/knowledge.md`: 3-tier table (Full Dewey, Graph-only, No Dewey) explaining what works at each tier and that Dewey is always optional
- [x] T010 [US1] Run `npm run build` to verify the new page builds without errors

**Checkpoint**: The Dewey getting-started guide exists, builds, and covers all sections from the quickstart outline.

---

## Phase 3: User Story 2 - Updated Workflow Pages (Priority: P1)

**Goal**: All 4 role-specific guides and the common workflows page reference Dewey's role in the hero lifecycle.

**Independent Test**: Visit each updated page in the dev server. Verify Dewey is mentioned in the context of the hero's role. Verify no page implies Dewey is required.

- [x] T011 [P] [US2] Update `content/docs/getting-started/common-workflows.md`: add a "Knowledge Context" subsection after the "Swarm Delegation" section explaining that Dewey provides semantic context to all heroes during their stages. Mention the 3-tier degradation briefly. Link to the Dewey getting-started guide
- [x] T012 [P] [US2] Update `content/docs/getting-started/developer.md`: add a "Knowledge Retrieval with Dewey" subsection (or expand the existing "Swarm Delegation" section) explaining how Cobalt-Crush uses `dewey_semantic_search` for toolstack API docs and implementation patterns from other repos. Include 2-3 example queries. Note the fallback behavior
- [x] T013 [P] [US2] Update `content/docs/getting-started/product-owner.md`: add a "Knowledge Retrieval with Dewey" subsection explaining how Muti-Mind uses Dewey for cross-repo backlog patterns, past acceptance criteria, and issue discovery across whitelisted repos
- [x] T014 [P] [US2] Update `content/docs/getting-started/product-manager.md`: add a "Knowledge Retrieval with Dewey" subsection explaining how Mx F uses Dewey for cross-repo velocity trends, retrospective outcomes, and coaching patterns
- [x] T015 [P] [US2] Update `content/docs/getting-started/tester.md`: add a "Knowledge Retrieval with Dewey" subsection explaining how Gaze uses Dewey for test quality patterns, CRAP score baselines from other repos, and known failure modes
- [x] T016 [US2] Run `npm run build` to verify all updated pages build without errors

**Checkpoint**: All 5 pages reference Dewey with role-specific context and fallback information.

---

## Phase 4: User Story 3 - Dewey Tool Page (Priority: P2)

**Goal**: The team section has a Dewey page describing its architecture, capabilities, and ecosystem role.

**Independent Test**: Navigate to `/docs/team/dewey/` in the dev server. Verify it explains what Dewey is, its relationship to graphthulhu, query capabilities, content sources, embedding model, and how heroes use it.

- [x] T017 [US3] Create `content/docs/team/dewey.md` with frontmatter: title "Dewey", description "Semantic knowledge layer for AI agent context", lead, date, weight 70, toc true, draft false
- [x] T018 [US3] Write the "What Dewey Does" section in `content/docs/team/dewey.md`: semantic knowledge layer combining structured knowledge graph with vector-based semantic search. Mention the graphthulhu heritage (hard fork, superset of capabilities)
- [x] T019 [US3] Write the "Query Capabilities" section in `content/docs/team/dewey.md`: structured queries (keyword search, tag lookup, wikilink traversal, property queries) and semantic queries (conceptual similarity, similar documents, filtered semantic search). Use the MCP tool names from Spec 014 contracts
- [x] T020 [US3] Write the "Content Sources" section in `content/docs/team/dewey.md`: local disk (Markdown files), GitHub API (issues, PRs, READMEs from whitelisted repos), web crawl (toolstack documentation). Explain the pluggable architecture
- [x] T021 [US3] Write the "Embedding Model" section in `content/docs/team/dewey.md`: IBM Granite (Apache 2.0, permissible training data, 63 MB model). Explain why enterprise licensing provenance matters. Note the model is configurable
- [x] T022 [US3] Write the "How Heroes Use Dewey" section in `content/docs/team/dewey.md`: brief per-hero overview table (PO: backlog patterns, Dev: toolstack docs, Tester: quality baselines, Reviewer: review patterns, Manager: velocity trends). Link to each hero's getting-started page
- [x] T023 [US3] Run `npm run build` to verify the tool page builds without errors

**Checkpoint**: The Dewey tool page exists with all sections from the quickstart outline.

---

## Phase 5: User Story 4 - Navigation Updates (Priority: P2)

**Goal**: New pages appear in the sidebar navigation with logical ordering.

**Independent Test**: Navigate the dev server sidebar. Verify the Dewey getting-started guide appears in the getting-started section and the Dewey tool page appears in the team section.

- [x] T024 [US4] Verify sidebar ordering in `content/docs/getting-started/knowledge.md` frontmatter: weight 80 should place it after common-workflows.md (weight 70). Adjust if needed after visual check with `npm run dev`
- [x] T025 [US4] Verify sidebar ordering in `content/docs/team/dewey.md` frontmatter: weight 70 should place it after mx-f.md (weight 60). Adjust if needed after visual check with `npm run dev`
- [x] T026 [US4] Update `content/docs/getting-started/_index.md`: add a link to the Dewey knowledge retrieval guide in the page list, consistent with how other guides are listed
- [x] T027 [US4] Update `content/docs/team/_index.md`: add Dewey as an infrastructure tool below the heroes listing. Do not change the "five AI agent personas" description -- Dewey is infrastructure, not a persona. Add a brief description and link.
- [x] T028 [US4] Check `config/_default/menus/menus.en.toml` -- if top-level navigation entries are needed for the new pages, add them. If Doks auto-generates sidebar from content structure, no menu changes needed

**Checkpoint**: Both new pages appear in the sidebar at logical positions.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Final build, link checking, dark mode verification.

- [x] T029 Run `npm run build` to verify the complete site builds with zero errors
- [x] T030 Run `npm run dev` and visually verify all new and updated pages: check that code blocks (YAML examples) have readable text in both light and dark mode, tables have visible borders/separators, and no text-on-same-color-background issues exist
- [x] T031 Check all internal links on new pages by clicking each one in the dev server: verify links to hero pages, common workflows, Dewey getting-started guide from the tool page, and section index pages all resolve. Report any 404s.
- [x] T032 Verify no page contains "Coming Soon", placeholder text, or TODO markers (Constitution Principle I: no placeholder content)
- [x] T033 Cross-reference all factual claims in new pages (MCP tool names, source types, embedding model name, CLI commands) against the Dewey repo's README and actual CLI help output. Verify no capability is overstated and no implemented feature is missing (Constitution Principle I).
- [x] T034 Update `AGENTS.md` Content Sources table: add a Dewey row with repository `unbound-force/dewey`, key files `README.md`, design paper

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies
- **US1 (Phase 2)**: Depends on Phase 1 -- new page
- **US2 (Phase 3)**: Can run in parallel with Phase 2 (different files)
- **US3 (Phase 4)**: Can run in parallel with Phases 2-3 (different file)
- **US4 (Phase 5)**: Depends on Phases 2+4 (pages must exist before navigation is verified)
- **Polish (Phase 6)**: Depends on all phases

### Parallel Opportunities

- T011, T012, T013, T014, T015 can all run in parallel (different files)
- Phases 2, 3, and 4 can run in parallel (different files)

---

## Implementation Strategy

### MVP First (US1 Only)

1. Phase 1: Verify build
2. Phase 2: Write Dewey getting-started guide
3. **STOP and VALIDATE**: `npm run build` passes, page renders in dev server
4. Core value delivered -- developers can learn about Dewey

### Incremental Delivery

1. US1 → Getting-started guide → Primary adoption enabler
2. US2 → Role guide updates → Heroes reference Dewey
3. US3 → Tool page → Deep reference for architecture
4. US4 → Navigation → Discoverability
5. Polish → Build, dark mode, links → Ship it

---

## Notes

- Content is derived from `../dewey-design-paper.md` and Spec 014 (meta repo). Adapt for website audience: less technical depth, more "why should I care"
- Use `##` headings (H2) for body content -- the `title` frontmatter generates H1
- Use ATX-style headings (`##`, not underlines)
- No placeholder or "Coming Soon" content (Constitution Principle I)
- Check Doks docs before adding any custom CSS or HTML (Constitution Principle II)
