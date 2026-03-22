---
description: "Task list for Getting Started Guides"
---

# Tasks: Getting Started Guides

**Input**: Design documents from `/specs/005-getting-started-guides/`
**Prerequisites**: plan.md (required), spec.md (required), research.md

**Tests**: Not applicable -- this is a static content feature. Validation is manual (`npm run build`, visual check, link verification).

**Organization**: Tasks grouped by user story (one guide page per story) for independent implementation.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to
- Include exact file paths in descriptions

## Path Conventions

- Content pages: `content/docs/getting-started/`
- Existing pages: `_index.md` (weight: 10), `quick-start.md` (weight: 20)

---

## Phase 1: Setup

**Purpose**: No setup needed -- the `content/docs/getting-started/` directory and `_index.md` already exist.

- [x] T001 Verify `content/docs/getting-started/_index.md` exists and has `weight: 10`. Read the existing `quick-start.md` to confirm its weight is 20. No changes to either file.

---

## Phase 2: Foundational

**Purpose**: No foundational tasks -- each guide is an independent Markdown file with no shared infrastructure.

---

## Phase 3: User Story 1 - Developer Getting Started Guide (Priority: P1)

**Goal**: Create a comprehensive developer guide covering prerequisites, daily workflow, Speckit, Swarm, Cobalt-Crush, and session lifecycle.

**Independent Test**: Run `npm run build`, then `npm run dev` and navigate to `/docs/getting-started/developer/`. Verify all 6 topic sections render with actionable content.

- [x] T002 [US1] Create `content/docs/getting-started/developer.md` with Hugo frontmatter (title: "Getting Started: Developer", description, lead, date: 2026-03-22, weight: 30, toc: true). Write the following H2 sections per FR-011: "Prerequisites" (covering `unbound setup` as a single command, what it installs, and `unbound doctor` for verification), "Daily Workflow" (the progression: `unbound doctor` check, open OpenCode, use Swarm for parallel tasks), "Working with Speckit" (the pipeline: `/speckit.specify` through `/speckit.implement`, when to use strategic vs tactical OpenSpec workflows), "Working with Swarm" (`/swarm "task"` command, file reservations via `swarmmail_reserve()`, session lifecycle), "Cobalt-Crush Persona" (what Cobalt-Crush is, convention packs in `.opencode/unbound/packs/`, how Gaze and Divisor feedback loops work), "Session Ritual" (start: `hive_ready()`, work: `/swarm` or direct coding, end: `hive_sync()` + `git push` -- "the plane is not landed until git push succeeds"). Include cross-reference links to the Common Workflows page and the Cobalt-Crush team page. Source content from upstream: `unbound-force/unbound-force/AGENTS.md`, `cobalt-crush-dev.md` agent file, swarmtools.ai quickstart, opencode.ai docs, `specs/011-doctor-setup/quickstart.md`.

- [x] T003 [US1] Run `npm run build` to verify `developer.md` builds without errors. Fix any frontmatter or Markdown syntax issues.

**Checkpoint**: Developer guide renders in sidebar at position after Quick Start with all 6 sections.

---

## Phase 4: User Story 2 - Common Workflows Reference (Priority: P1)

**Goal**: Create a cross-role reference page covering 4 end-to-end workflows.

**Independent Test**: Run `npm run build`, then navigate to `/docs/getting-started/common-workflows/`. Verify all 4 workflow sections render with role-attributed step-by-step flows.

- [x] T004 [P] [US2] Create `content/docs/getting-started/common-workflows.md` with Hugo frontmatter (title: "Common Workflows", description, lead, date: 2026-03-22, weight: 70, toc: true). Write the following H2 sections per FR-015: "New Feature (End-to-End)" (step-by-step flow: 1. PO defines backlog item via Muti-Mind, 2. Dev creates spec via `/speckit.specify`, 3. Dev runs clarify/plan/tasks pipeline, 4. Dev implements via `/speckit.implement` or `/cobalt-crush`, 5. Tester validates via Gaze quality analysis, 6. Reviewer approves via `/review-council` with 5 Divisor personas, 7. PO accepts the increment via Muti-Mind, 8. PM measures sprint metrics via Mx F -- identify the responsible role at each step), "Bug Fix (Tactical)" (OpenSpec flow: `/opsx-propose` to create the change, `/opsx-apply` to implement, `/review-council` to review, `/opsx-archive` to archive), "Code Review" (how to invoke `/review-council`, what each of the 5 Divisor personas evaluates: Guard=intent drift, Architect=conventions, Adversary=security, Testing=coverage, SRE=operations), "Environment Setup" (the one-time flow: install `unbound` via Homebrew, run `unbound setup`, run `unbound doctor` to verify, start first task). Include cross-reference links to each role-specific guide. Source content from `unbound-force-heroes/SKILL.md`, `specs/008-swarm-orchestration/spec.md`.

- [x] T005 [US2] Run `npm run build` to verify `common-workflows.md` builds without errors.

**Checkpoint**: Common Workflows page renders as the last item in Getting Started sidebar with all 4 workflows.

---

## Phase 5: User Story 3 - Tester Getting Started Guide (Priority: P2)

**Goal**: Create a tester guide covering Gaze commands, quality metrics, the review council, and CI integration.

**Independent Test**: Navigate to `/docs/getting-started/tester/`. Verify Gaze commands section, quality metrics section, review council section, and CI section all render.

- [x] T006 [P] [US3] Create `content/docs/getting-started/tester.md` with Hugo frontmatter (title: "Getting Started: Tester", description, lead, date: 2026-03-22, weight: 40, toc: true). Write the following H2 sections per FR-012: "Using Gaze" (the 4 core commands: `gaze analyze` detects side effects in Go functions across 30+ types, `gaze quality` assesses test quality via Contract Coverage and Over-Specification Score, `gaze crap` computes CRAP and GazeCRAP composite risk scores, `gaze report` generates AI-powered quality reports with multi-adapter support), "Quality Metrics" (what contract coverage means -- percentage of observable side effects covered by tests, what side effects are -- categorized P0-P4 from system exits to pure computations, what CRAP scores measure -- Change Risk Anti-Patterns combining complexity and coverage), "The Hero Workflow" (how Gaze quality reports feed into The Divisor's review as evidence, how Mx F uses quality metrics for sprint health), "Running the Review Council" (`/review-council` invocation, the 5 personas and what each checks), "Coverage Ratchets and CI" (how coverage thresholds are enforced in CI, the ratchet pattern that prevents coverage from dropping). Include link to Gaze project page at `/docs/projects/gaze/` and Common Workflows page. Source content from `gaze/README.md`, divisor agent files.

- [x] T007 [US3] Run `npm run build` to verify `tester.md` builds without errors.

**Checkpoint**: Tester guide renders in sidebar between Developer and Product Owner.

---

## Phase 6: User Story 4 - Product Owner Getting Started Guide (Priority: P2)

**Goal**: Create a product owner guide covering Muti-Mind, the hero lifecycle, and working with specifications.

**Independent Test**: Navigate to `/docs/getting-started/product-owner/`. Verify Muti-Mind section, hero lifecycle section, and specifications section all render.

- [x] T008 [P] [US4] Create `content/docs/getting-started/product-owner.md` with Hugo frontmatter (title: "Getting Started: Product Owner", description, lead, date: 2026-03-22, weight: 50, toc: true). Write the following H2 sections per FR-013: "Using Muti-Mind" (backlog management -- creating and prioritizing work items, priority scoring using multi-factor analysis, acceptance decisions -- reviewing completed work and deciding accept/reject, GitHub issue sync -- how Muti-Mind keeps backlog items synchronized with GitHub Issues), "Your Role in the Hero Lifecycle" (the 6-stage workflow from the PO perspective: stage 1 Define -- PO creates the backlog item and spec input, stage 5 Accept -- PO reviews the completed increment with quality data from Gaze and review verdicts from The Divisor, what to look for when accepting, what happens on rejection -- a new backlog item is created with rationale), "Working with Specifications" (how to read a Speckit spec -- the frontmatter, user stories, requirements, success criteria sections, how to provide acceptance criteria -- clear, testable Given/When/Then scenarios, how specs flow through the pipeline -- specify creates it, clarify refines it, plan designs it, tasks decompose it, implement builds it). Include link to Muti-Mind team page and Common Workflows. Source content from `muti-mind-po.md` agent file, `specs/004-muti-mind-architecture/`.

- [x] T009 [US4] Run `npm run build` to verify `product-owner.md` builds without errors.

**Checkpoint**: Product Owner guide renders in sidebar between Tester and Product Manager.

---

## Phase 7: User Story 5 - Product Manager Getting Started Guide (Priority: P3)

**Goal**: Create a product manager guide covering Mx F, dashboards, coaching, and retrospectives.

**Independent Test**: Navigate to `/docs/getting-started/product-manager/`. Verify Mx F section, dashboard section, coaching section, and integration section all render.

- [x] T010 [P] [US5] Create `content/docs/getting-started/product-manager.md` with Hugo frontmatter (title: "Getting Started: Product Manager", description, lead, date: 2026-03-22, weight: 60, toc: true). Write the following H2 sections per FR-014: "Using Mx F" (the `mxf` CLI and its 7 subcommands: `collect` gathers metrics, `metrics` displays current data, `impediment` tracks blockers, `dashboard` shows the TUI overview, `sprint` manages sprint lifecycle, `standup` runs daily standups, `retro` facilitates retrospectives), "Reading the Dashboard" (what velocity metrics represent -- story points or task throughput per sprint, what quality trends show -- Gaze scores over time and coverage trajectory, what hero utilization means -- which heroes are active and how workload distributes), "Coaching and Retrospectives" (how Mx F coaching identifies patterns and provides actionable suggestions, how retrospectives are structured -- what went well, what needs improvement, action items, how impediments are tracked and escalated), "Integration with Other Heroes" (how Mx F metrics connect to Muti-Mind -- backlog velocity informs priority decisions, how Mx F connects to Gaze -- quality metrics feed into sprint health, the measure stage in the hero lifecycle -- Mx F captures a metrics snapshot after each completed workflow). Include link to Mx F team page and Common Workflows. Source content from `mx-f-coach.md` agent file, `specs/007-mx-f-architecture/`.

- [x] T011 [US5] Run `npm run build` to verify `product-manager.md` builds without errors.

**Checkpoint**: Product Manager guide renders in sidebar between Product Owner and Common Workflows.

---

## Phase 8: Polish & Cross-Cutting Concerns

**Purpose**: Cross-reference validation, build verification, and final review.

- [x] T012 Update `content/docs/getting-started/_index.md` to add links to all 5 new guide pages in the "Get Started" section at the bottom, alongside the existing Quick Start link. Preserve existing content.

- [x] T013 Run `npm run build` to verify the full site builds with zero errors after all 5 guides are added.

- [x] T014 Verify cross-reference links: check that all internal links between guide pages (developer to common-workflows, tester to gaze project page, PO to muti-mind team page, PM to mx-f team page, common-workflows to all role guides) resolve to valid pages. Fix any broken links.

- [x] T015 Verify sidebar ordering: run `npm run dev` and confirm the Getting Started sidebar shows pages in order: What is Unbound Force?, Quick Start, Developer, Tester, Product Owner, Product Manager, Common Workflows.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: Verification only -- no changes needed
- **Foundational (Phase 2)**: None -- each guide is independent
- **US1 Developer (Phase 3)**: No dependencies -- start immediately
- **US2 Common Workflows (Phase 4)**: No dependencies -- can run parallel with US1
- **US3 Tester (Phase 5)**: No dependencies -- can run parallel with US1/US2
- **US4 Product Owner (Phase 6)**: No dependencies -- can run parallel
- **US5 Product Manager (Phase 7)**: No dependencies -- can run parallel
- **Polish (Phase 8)**: Depends on all user stories being complete

### User Story Dependencies

All 5 user stories are fully independent -- each creates a separate Markdown file with no shared state. They can be implemented in any order or in parallel.

### Parallel Opportunities

- T002, T004, T006, T008, T010 are all `[P]` -- they create different files and can run in parallel
- All 5 guide pages can be written simultaneously by different workers
- Build verification tasks (T003, T005, T007, T009, T011) depend on their respective guide page

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. T001: Verify setup
2. T002-T003: Developer guide
3. **STOP and VALIDATE**: Site builds, developer guide renders correctly

### Incremental Delivery

1. Developer guide (P1) -- most critical, covers largest tool surface
2. Common Workflows (P1) -- ties all roles together
3. Tester guide (P2) -- bridges Gaze CLI to workflow context
4. Product Owner guide (P2) -- enables PO engagement
5. Product Manager guide (P3) -- completes the set
6. Polish -- cross-references, sidebar, landing page

### Parallel Team Strategy

All 5 guides can be written in parallel by different workers since they are independent Markdown files.

---

## Notes

- All content MUST be sourced from upstream repositories (Constitution Principle I: Content Accuracy)
- No custom HTML, CSS, or JavaScript (Constitution Principle II: Minimal Footprint)
- Each page MUST serve a clear audience with actionable guidance (Constitution Principle III: Visitor Clarity)
- Use fenced code blocks with language identifiers for all CLI command examples
- Start body content with H2 headings (H1 is generated from frontmatter title)
- No placeholder or "Coming Soon" content (zero-waste mandate)
