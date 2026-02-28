# Tasks: Documentation Content Pages

**Input**: Design documents from `/specs/002-documentation-content/`
**Prerequisites**: plan.md, spec.md, research.md, data-model.md, quickstart.md

**Tests**: No test tasks — the spec does not request automated tests. Validation is manual: `npm run build` + visual verification via `npm run dev`.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Create directory structure and docs root index required by all sections

- [x] T001 Create directory structure for all documentation sections: `mkdir -p content/docs/getting-started content/docs/projects content/docs/team content/docs/contributing`
- [x] T002 Create docs root index page in `content/docs/_index.md` with frontmatter: title "Documentation", description of Unbound Force docs, lead text introducing the documentation, date 2026-02-23, draft false, weight 1, toc false. Body content: one brief sentence such as "Explore the documentation to learn about Unbound Force, its projects, and how to get started." Do not add redirect logic — Hugo renders the first section (Getting Started) automatically when navigating to `/docs/`.

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Fetch upstream source content that multiple user stories depend on

**Note**: This is a content-only feature. The "foundational" work is gathering the source material that US1, US2, US3, and US4 all adapt from. Since the plan already completed this research (see `research.md`), this phase is effectively pre-satisfied. The source content is documented in:

- `research.md` R1 — `unbound-force/unbound-force.md` overview (used by US1, US3)
- `research.md` R2 — Gaze `/gaze` OpenCode command details (used by US2)
- `research.md` R3 — Persona descriptions and image mappings (used by US3)
- `research.md` R4 — Contributing conventions from AGENTS.md (used by US4)

- [x] T003 Verify source content availability: confirm `research.md` contains resolved content for all user stories. Read `specs/002-documentation-content/research.md` and confirm R1 through R8 all have decisions (no NEEDS CLARIFICATION remaining). If any are unresolved, stop and escalate.

**Checkpoint**: Source content verified — user story implementation can now begin

---

## Phase 3: User Story 1 — Getting Started Guide (Priority: P1) MVP

**Goal**: First-time visitors who click "Get Started" on the homepage land on a clear explanation of Unbound Force and actionable steps to begin using Specify, OpenCode, and Swarm.

**Independent Test**: Click "Get Started" on the homepage (or navigate to `/docs/getting-started/`). The Getting Started section loads with a "What is Unbound Force?" overview and a Quick Start sub-page covering all three tools.

**Source**: `unbound-force/unbound-force.md` overview section (R1), tool documentation for Specify/OpenCode/Swarm (R1)

### Implementation for User Story 1

- [x] T004 [US1] Create Getting Started section index in `content/docs/getting-started/_index.md`. Frontmatter: title "What is Unbound Force?", description about the AI agent swarm for software engineering, lead text introducing the concept, date 2026-02-23, draft false, weight 10, toc true. Body content adapted from `unbound-force/unbound-force.md` overview — explain what Unbound Force is (superhero-themed AI agent swarm), the three tools (Specify, OpenCode, Swarm), and that each hero can be more than just instructions (LSP, MCP, tooling, commands, plugins). Frame for website audience: "why should I care" not raw technical specs. Must satisfy FR-001, FR-011, FR-012, FR-016.
- [x] T005 [US1] Create Quick Start page in `content/docs/getting-started/quick-start.md`. Frontmatter: title "Quick Start", description about getting started with the tools, lead text, date 2026-02-23, draft false, weight 20, toc true. Body content covers all three tools equally per R1 research — present as a layered stack (Agent: OpenCode, Planning: Specify, Coordination: Swarm). For each tool: what it does (1-2 sentences), how to install it, and a brief usage example. Include links to each tool's website/repo. Frame as "why should I care" — not a raw installation guide. Must satisfy FR-002, FR-011, FR-012, FR-016.

**Checkpoint**: Getting Started section complete — verify by navigating to `/docs/getting-started/` and confirming both pages render with sidebar showing index + Quick Start in order

---

## Phase 4: User Story 2 — Gaze Project Page (Priority: P1)

**Goal**: Developers interested in test quality can read about the Gaze project, understand what it does, see how to use it via the `/gaze` OpenCode command, and find honest information about its current limitations.

**Independent Test**: Navigate to `/docs/projects/`. The projects overview lists Gaze. Click through to the Gaze page — it explains side effect detection, Contract Coverage, GazeCRAP, the `/gaze` command usage, architecture, limitations, and links to both the GitHub repo and the blog article.

**Source**: Gaze README (R2), `/gaze` OpenCode command research (R2), known limitations (R7), blog cross-link (R2)

### Implementation for User Story 2

- [x] T006 [P] [US2] Create Projects section index in `content/docs/projects/_index.md`. Frontmatter: title "Projects", description about Unbound Force projects, lead text, date 2026-02-23, draft false, weight 10, toc false. Body content: brief overview of the Projects section, list Gaze as the current project with a 1-2 sentence description and link to its sub-page. Must satisfy FR-003, FR-011, FR-012.
- [x] T007 [US2] Create Gaze project page in `content/docs/projects/gaze.md`. Frontmatter: title "Gaze", description about test quality analysis via side effect detection for Go, lead text, date 2026-02-23, draft false, weight 20, toc true. Body content adapted from Gaze README and R2 research, structured as:
  - **What Gaze Does**: Side effect detection, Contract Coverage, Over-Specification Score, GazeCRAP — adapted from README, not copied verbatim. Frame around the problem it solves (line coverage is insufficient).
  - **Key Metrics**: Contract Coverage (% of contractual effects asserted on), Over-Specification Score (count of incidental assertions), GazeCRAP (composite risk using contract coverage instead of line coverage). Brief explanations with the formulas.
  - **Using Gaze** (via `/gaze` OpenCode command): Install Gaze (`brew install unbound-force/tap/gaze` or `go install`), run `gaze init` in a Go project, then use `/gaze` inside OpenCode. Document the 3 modes: full (default), crap, quality — with example commands. Mention `/classify-docs` as a companion command. Do NOT document CLI commands directly — present only the OpenCode integration.
  - **Architecture**: Brief overview of the internal structure (analysis, taxonomy, classify, config, loader, report, crap, quality, docscan, scaffold) — adapted for website audience, not raw directory listing.
  - **Current Limitations**: Honest list from R7 — direct function body only, P3-P4 not yet detected, assertion mapping ~74% accuracy (target 90%), no CGo/unsafe, single package loading. Frame constructively.
  - **Links**: Prominent link to `https://github.com/unbound-force/gaze`, link to blog article at `/blog/why-contract-coverage/` for deeper technical explanation.
  - Must satisfy FR-004, FR-005, FR-011, FR-012, FR-015, FR-016.

**Checkpoint**: Projects section complete — verify Gaze page renders, `/gaze` command usage is clear, limitations are listed, blog link works, GitHub link is present

---

## Phase 5: User Story 3 — Team Persona Pages (Priority: P2)

**Goal**: Visitors curious about the AI agent swarm can browse team personas, see trading card images, and understand each role's purpose within the Unbound Force swarm.

**Independent Test**: Navigate to `/docs/team/`. The overview shows the team composite image and lists all 5 personas. Click each persona — every page loads with a trading card image and accurate description adapted from `unbound-force.md`.

**Source**: `unbound-force/unbound-force.md` Heroes section (R3), trading card images in `static/images/team/` (R3, R6)

### Implementation for User Story 3

- [x] T008 [US3] Create Team section index in `content/docs/team/_index.md`. Frontmatter: title "The Team", description about the Unbound Force agent personas, lead text introducing the Heroes concept, date 2026-02-23, draft false, weight 10, toc true. Body content: introduce "The Heroes" — Unbound Force's superhero-themed AI agent swarm. Display the team composite image using `![The Unbound Force — A.G.I. Development Squad](/images/team/the-unbound-force.jpeg)`. List all 5 personas with their role, tagline, and a brief 1-2 sentence description linking to their individual page. Must satisfy FR-006, FR-011, FR-012, FR-016.
- [x] T009 [P] [US3] Create Muti-Mind persona page in `content/docs/team/muti-mind.md`. Frontmatter: title "Muti-Mind", description about the Product Owner persona, lead "The Vision Keeper and Prioritization Engine", date 2026-02-23, draft false, weight 20, toc true. Display trading card: `![Muti-Mind — Product Owner trading card](/images/team/multi-mind.png)`. Body adapted from `unbound-force.md` Muti-Mind section — cover vision keeping, prioritization, backlog ownership, acceptance authority, and the "Voice of the User" concept. Explain how Muti-Mind serves developers (clarification), testers (acceptance criteria), and reviewers (baseline for review). Adapt for website audience. Must satisfy FR-007, FR-008, FR-011, FR-012, FR-016.
- [x] T010 [P] [US3] Create Cobalt-Crush persona page in `content/docs/team/cobalt-crush.md`. Frontmatter: title "Cobalt-Crush", description about the Developer persona, lead "The Engineering Core and Adaptive Implementation Engine", date 2026-02-23, draft false, weight 30, toc true. Display trading card: `![Cobalt-Crush — Developer trading card](/images/team/cobalt-crush.png)`. Body adapted from `unbound-force.md` — cover high-velocity implementation, CI/CD focus, architectural adherence, tight feedback loops with Gaze (tester), and technical problem solving. Must satisfy FR-007, FR-008, FR-011, FR-012, FR-016.
- [x] T011 [P] [US3] Create Gaze (Tester) persona page in `content/docs/team/gaze-tester.md`. Frontmatter: title "Gaze", description about the Tester persona, lead "The Quality Sentinel and Predictive Validation Engine", date 2026-02-23, draft false, weight 40, toc true. Display trading card: `![Gaze — Tester trading card](/images/team/gaze.png)`. Body adapted from `unbound-force.md` — cover proactive test strategies, CI/CV pipeline ownership, intelligent defect detection, risk-based testing, and performance/security profiling. Note: this is the Gaze persona (the tester role), distinct from the Gaze project page (the tool). Must satisfy FR-007, FR-008, FR-011, FR-012, FR-016.
- [x] T012 [P] [US3] Create The Divisor persona page in `content/docs/team/the-divisor.md`. Frontmatter: title "The Divisor", description about the PR Reviewer Council persona, lead "The Architectural Conscience and Code Integrity Guardian", date 2026-02-23, draft false, weight 50, toc true. Display trading card: `![The Divisor — PR Reviewer Council trading card](/images/team/the-divisor.png)`. Body adapted from `unbound-force.md` — MUST describe the three sub-personas: The Guard (intent and cohesion), The Architect (structure and sustainability), The Adversary (resilience and security). Explain the collective approval requirement — no outstanding REQUEST CHANGES from any persona. Cover Zero-Waste Mandate and Neighborhood Rule references. Must satisfy FR-007, FR-008, FR-009, FR-011, FR-012, FR-016.
- [x] T013 [P] [US3] Create Mx F persona page in `content/docs/team/mx-f.md`. Frontmatter: title "Mx F", description about the Manager persona, lead "The Flow Facilitator and Continuous Improvement Coach", date 2026-02-23, draft false, weight 60, toc true. Display trading card: `![Mx F — Manager trading card](/images/team/mx-f.png)`. Body adapted from `unbound-force.md` — cover servant leadership, coaching via reflective questions (not direct solutions), obstacle removal, flow optimization, process stewardship, retrospectives, and capacity monitoring. Must satisfy FR-007, FR-008, FR-011, FR-012, FR-016.

**Checkpoint**: Team section complete — verify all 6 pages render, composite image on index, trading card on each persona page, sidebar shows overview + 5 personas in order (Muti-Mind, Cobalt-Crush, Gaze, The Divisor, Mx F)

---

## Phase 6: User Story 4 — Contributing Guide (Priority: P2)

**Goal**: Developers who want to contribute to Unbound Force projects find clear instructions for reporting issues, submitting PRs, following conventions, and using the Speckit workflow.

**Independent Test**: Navigate to `/docs/contributing/`. The page explains how to contribute with actionable steps and links to GitHub.

**Source**: AGENTS.md conventions (R4), Speckit workflow (R4)

### Implementation for User Story 4

- [x] T014 [US4] Create Contributing section index in `content/docs/contributing/_index.md`. Frontmatter: title "Contributing", description about how to contribute to Unbound Force, lead text, date 2026-02-23, draft false, weight 10, toc true. Body content covering:
  - **Reporting Issues**: Link to GitHub issues for the relevant repositories
  - **Submitting Pull Requests**: Standard GitHub PR workflow, mention branch naming convention (e.g., `###-feature-name`)
  - **Conventional Commits**: List the valid types: `feat:`, `fix:`, `docs:`, `chore:`, `style:`, `refactor:`, `ci:` with brief examples
  - **Speckit Workflow**: Explain that all non-trivial work goes through the pipeline: constitution → specify → clarify → plan → tasks → analyze → checklist → implement. Link to the Specify project.
  - **Links**: GitHub organization (`https://github.com/unbound-force`), website repo, Gaze repo
  - Must satisfy FR-010, FR-011, FR-012, FR-015.

**Checkpoint**: Contributing section complete — verify page renders with GitHub links and all convention details

---

## Phase 7: User Story 5 — Navigation & Configuration (Priority: P1)

**Goal**: All documentation sections appear in the sidebar in the correct order, the "Get Started" button works on the homepage, and the full site builds successfully.

**Independent Test**: From the homepage, click "Get Started" — lands on Getting Started. Sidebar shows all four sections in order. Every page is reachable within two clicks. `npm run build` succeeds with zero errors.

**Source**: Navigation config (R5), "Get Started" button (R8), data-model.md validation rules

### Implementation for User Story 5

- [x] T015 [US5] Re-enable "Get Started" navbar button in `config/_default/params.toml`: change `navBarButton = false` to `navBarButton = true` on line 23. Must satisfy US5 acceptance scenario 1 and R8.
- [x] T016 [US5] Verify `config/_default/menus/menus.en.toml` has all 4 `[[docs]]` entries with correct weights: Getting Started (10), Projects (20), The Team (30), Contributing (40). Read the file and confirm — do NOT modify if already correct (it should be per R5). Must satisfy FR-014.
- [x] T017 [US5] Run `npm run build` and verify zero errors. If build fails, fix the issue before proceeding. Must satisfy SC-003.

**Checkpoint**: Navigation verified — sidebar order correct, "Get Started" button active, build succeeds

---

## Phase 8: Polish & Cross-Cutting Concerns

**Purpose**: Final validation across all sections and stories

- [x] T018 Verify all 12 content pages have complete YAML frontmatter: check every `.md` file in `content/docs/` for required fields (title, description, lead, date, draft: false, weight, toc). Must satisfy SC-007.
- [x] T019 Verify no placeholder content exists: search all content pages for "Coming Soon", "TODO", "TBD", "Lorem ipsum", or similar placeholder text. Must satisfy SC-006.
- [x] T020 Verify all internal links are valid: check cross-references between pages — specifically the Gaze project page link to `/blog/why-contract-coverage/`, the Getting Started links to tool websites, the Contributing links to GitHub repos. Must satisfy FR-015.
- [x] T021 Verify sidebar ordering: run `npm run dev` and confirm sidebar shows sections in order (Getting Started, Projects, Team, Contributing) with sub-pages in correct weight order within each section. Must satisfy SC-002, SC-008.
- [x] T022 Verify dark mode rendering: with `npm run dev` running, toggle dark mode and verify all 12 pages render correctly — text readable, images display properly, no broken styling.
- [x] T023 Final build verification: run `npm run build` one last time to confirm the complete site builds with zero errors after all content and config changes. Must satisfy SC-003.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — start immediately
- **Foundational (Phase 2)**: Depends on Setup — verifies source content
- **US1 Getting Started (Phase 3)**: Depends on Phase 2 — can start immediately after
- **US2 Gaze Project (Phase 4)**: Depends on Phase 2 — **can run in parallel with Phase 3**
- **US3 Team Personas (Phase 5)**: Depends on Phase 2 — **can run in parallel with Phases 3-4**
- **US4 Contributing (Phase 6)**: Depends on Phase 2 — **can run in parallel with Phases 3-5**
- **US5 Navigation (Phase 7)**: Depends on Phases 3-6 (all content must exist before nav verification)
- **Polish (Phase 8)**: Depends on Phase 7 (all content and config complete)

### User Story Dependencies

- **US1 (P1)**: Independent — no dependencies on other user stories
- **US2 (P1)**: Independent — no dependencies on other user stories. Blog page `/blog/why-contract-coverage/` already exists.
- **US3 (P2)**: Independent — no dependencies on other user stories
- **US4 (P2)**: Independent — no dependencies on other user stories
- **US5 (P1)**: Depends on US1-US4 completion (verifies navigation across all sections)

### Within Each User Story

- Section `_index.md` before sub-pages (Hugo needs it for section recognition)
- All persona pages (T009-T013) can run in parallel after the team index (T008)

### Parallel Opportunities

- **Phase 3 + Phase 4 + Phase 5 + Phase 6** can all run in parallel after Phase 2
- Within Phase 4: T006 (projects index) and T007 (gaze page) — T006 can run in parallel with anything in other phases
- Within Phase 5: T009, T010, T011, T012, T013 (all 5 persona pages) can run in parallel after T008 (team index)

---

## Parallel Example: User Story 3 (Team Personas)

```text
# First: create the team index (required for section recognition)
Task: T008 — Create team section index in content/docs/team/_index.md

# Then: launch all 5 persona pages in parallel (different files, no dependencies)
Task: T009 — Create Muti-Mind page in content/docs/team/muti-mind.md
Task: T010 — Create Cobalt-Crush page in content/docs/team/cobalt-crush.md
Task: T011 — Create Gaze (Tester) page in content/docs/team/gaze-tester.md
Task: T012 — Create The Divisor page in content/docs/team/the-divisor.md
Task: T013 — Create Mx F page in content/docs/team/mx-f.md
```

## Parallel Example: All P1 User Stories

```text
# After Phase 2 (foundational) completes, launch US1 and US2 simultaneously:

# US1 — Getting Started (sequential within story)
Task: T004 — Create getting-started index
Task: T005 — Create quick-start page

# US2 — Gaze Project (T006 in parallel, T007 after)
Task: T006 — Create projects index
Task: T007 — Create gaze project page
```

---

## Implementation Strategy

### MVP First (User Stories 1 + 2 Only)

1. Complete Phase 1: Setup (T001-T002)
2. Complete Phase 2: Foundational (T003)
3. Complete Phase 3: US1 Getting Started (T004-T005)
4. Complete Phase 4: US2 Gaze Project (T006-T007)
5. **STOP and VALIDATE**: Run `npm run build`, verify both sections render, "Get Started" button works
6. This delivers the two P1 content sections — the homepage CTA path and the flagship project page

### Incremental Delivery

1. Setup + Foundational → Framework ready
2. Add US1 Getting Started → Homepage CTA works → P1 partial
3. Add US2 Gaze Project → Flagship project documented → P1 complete
4. Add US3 Team Personas → Agent swarm explained → P2 partial
5. Add US4 Contributing → Community onboarding complete → P2 complete
6. US5 Navigation + Polish → Full validation → Feature complete

### Parallel Strategy

With multiple agents:

1. All complete Setup + Foundational together
2. Once Phase 2 is done:
   - Agent A: US1 Getting Started (2 pages)
   - Agent B: US2 Gaze Project (2 pages)
   - Agent C: US3 Team Personas (6 pages — highest page count, benefits most from parallel persona creation)
   - Agent D: US4 Contributing (1 page)
3. All stories complete independently → Phase 7 navigation verification → Phase 8 polish

---

## Notes

- [P] tasks = different files, no dependencies on incomplete tasks
- [Story] label maps task to specific user story for traceability
- Each user story is independently completable and testable
- No automated tests — validation is `npm run build` + visual verification via `npm run dev`
- Commit after each phase or logical group of tasks
- All content must be adapted for website audience — never copy READMEs verbatim (FR-016)
- All content must be factually accurate against source repos — never fabricate features (Constitution I)
- Source content references are in `specs/002-documentation-content/research.md` (R1-R8)
