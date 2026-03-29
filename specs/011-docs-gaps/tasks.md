# Tasks: Documentation Gaps

**Input**: Design documents from `/specs/011-docs-gaps/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, quickstart.md

**Tests**: No automated tests -- this is a static documentation site. Validation is `npm run build` + visual verification.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: No setup needed -- all target files either already exist or are new Markdown files in an existing directory. The Doks theme auto-generates sidebar navigation from frontmatter weights.

_(No tasks in this phase)_

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: No foundational blocking work. All user stories modify independent files or sections and can proceed immediately. The only potential dependency is US1 (constitution) and US2 (artifacts) cross-linking to each other, but since both are new pages, their cross-links can be written at creation time referencing the expected URL paths.

_(No tasks in this phase)_

**Checkpoint**: All user stories can begin immediately.

---

## Phase 3: User Story 1 — Constitution and Governance Page (Priority: P1)

**Goal**: Create a new page documenting the 4 organizational principles with MUST/SHOULD rules, governance model, hero constitution alignment, and Constitution Check process.

**Independent Test**: Navigate to the constitution page from the sidebar. Verify all 4 principles with MUST/SHOULD rules. Verify governance section covers amendments, versioning, supremacy clause, hero alignment, and conflict resolution.

### Implementation for User Story 1

- [x] T001 [US1] Create content/docs/getting-started/constitution.md with YAML frontmatter (title: "Constitution", description, lead, date, draft: false, weight: 85, toc: true). Include an introduction section explaining what the constitution is, its authority as the highest-authority document in the organization, and why it matters for contributors and hero developers. Source: research.md R1.
- [x] T002 [US1] Add Principle I: Autonomous Collaboration section to content/docs/getting-started/constitution.md. Include: concept description (artifact-based communication, no runtime coupling), MUST rules (no synchronous dependencies, self-describing artifacts with metadata, use envelope format), SHOULD rules (publish to well-known locations), and rationale paragraph. Cross-link to the artifacts page for envelope format details. Source: org constitution v1.1.0, research.md R1.
- [x] T003 [US1] Add Principle II: Composability First section to content/docs/getting-started/constitution.md. Include: concept description (independent installability, extension points, additive value), MUST rules (standalone core value, extension points over modification, additive combination value), SHOULD rules (auto-detect peers), and rationale paragraph. Source: org constitution v1.1.0, research.md R1.
- [x] T004 [US1] Add Principle III: Observable Quality section to content/docs/getting-started/constitution.md. Include: concept description (machine-parseable output, provenance, reproducible evidence), MUST rules (JSON format minimum, provenance metadata, automated evidence, stable formats), SHOULD rules (conform to shared schemas), and rationale paragraph. Source: org constitution v1.1.0, research.md R1.
- [x] T005 [US1] Add Principle IV: Testability section to content/docs/getting-started/constitution.md. Include: concept description (isolation, observable side effects, coverage strategy), MUST rules (test without external services, verify side effects not internals, define coverage strategy in plans, enforce coverage ratchets, missing coverage = CRITICAL finding), and rationale paragraph. Source: org constitution v1.1.0, research.md R1.
- [x] T006 [US1] Add Hero Constitution Alignment section to content/docs/getting-started/constitution.md. Include: `parent_constitution` reference requirement, additive-only hero principles (must not contradict org-level MUST rules), and amendment review obligations (all hero constitutions reviewed when org constitution changes). Source: org constitution v1.1.0, research.md R1.
- [x] T007 [US1] Add Governance section to content/docs/getting-started/constitution.md. Include: supremacy clause (org constitution prevails over hero constitutions), amendment process (PR + review + migration plan), semantic versioning scheme (MAJOR: principle removal or incompatible redefinition; MINOR: new principle or expanded guidance; PATCH: clarifications and wording), compliance review at planning phases, and conflict resolution guidance (explicit documentation required, no implicit priority). Source: org constitution v1.1.0, research.md R1.
- [x] T008 [US1] Add Constitution Check section to content/docs/getting-started/constitution.md. Describe how the Constitution Check gates the planning phase: every plan must pass a check against all active principles before implementation begins. Violations are CRITICAL severity and non-negotiable. Mention the `/constitution-check` command and how OpenSpec proposals include a Constitution Alignment section. Cross-link to contributing page for context on the spec pipeline. Source: org constitution v1.1.0, research.md R1.

**Checkpoint**: Constitution page is complete and accessible in sidebar. `npm run build` passes.

---

## Phase 4: User Story 2 — Artifact Types and Envelope Format Page (Priority: P1)

**Goal**: Create a new page documenting the 7 inter-hero artifact types, the standard envelope format, and the lifecycle stage mapping.

**Independent Test**: Navigate to the artifacts page from the sidebar. Verify all 7 artifact types with producer/consumer mappings. Verify envelope format fields. Verify lifecycle stage mapping.

### Implementation for User Story 2

- [x] T009 [P] [US2] Create content/docs/getting-started/artifacts.md with YAML frontmatter (title: "Hero Artifacts", description, lead, date, draft: false, weight: 75, toc: true). Include an introduction section explaining that heroes communicate through artifacts (Constitution Principle I: Autonomous Collaboration), artifacts are files with a standard envelope format, and link to the constitution page. Source: research.md R2.
- [x] T010 [US2] Add Envelope Format section to content/docs/getting-started/artifacts.md. Describe the 7 standard fields: `hero` (producing hero name), `version` (hero version string), `timestamp` (ISO 8601), `artifact_type` (one of the 7 defined types), `schema_version` (semver of the artifact schema), `context` (branch, commit SHA, backlog item reference), `payload` (artifact-specific data object). Explain that each artifact is self-describing per Constitution Principle I. Source: AGENTS.md, research.md R2.
- [x] T011 [US2] Add Artifact Types section to content/docs/getting-started/artifacts.md. Create a table listing all 7 artifact types with columns: Type, Producer, Consumers, Description. Include: `quality-report` (Gaze -> Mx F, Muti-Mind, Cobalt-Crush; test results, coverage, CRAP scores), `review-verdict` (The Divisor -> Mx F, Cobalt-Crush, Muti-Mind; APPROVE/REQUEST CHANGES with per-persona findings), `backlog-item` (Muti-Mind -> Mx F, Cobalt-Crush; feature/bug descriptions with priority scores), `acceptance-decision` (Muti-Mind -> Mx F, Cobalt-Crush; ACCEPT/REJECT/CONDITIONAL), `metrics-snapshot` (Mx F -> Muti-Mind; velocity, quality trends), `coaching-record` (Mx F -> All heroes; learning feedback, recommendations), `workflow-record` (Swarm Orchestration -> Mx F, Muti-Mind; complete workflow trace). Source: AGENTS.md, research.md R2.
- [x] T012 [US2] Add Lifecycle Stage Mapping section to content/docs/getting-started/artifacts.md. Create a table or list showing which artifacts are produced at each of the 6 hero lifecycle stages: Define -> `backlog-item`; Implement -> (code, not envelope-formatted); Validate -> `quality-report`; Review -> `review-verdict`; Accept -> `acceptance-decision`; Reflect -> `metrics-snapshot`, `coaching-record`, `workflow-record`. Cross-link to the common-workflows page for the full lifecycle description. Source: research.md R2.

**Checkpoint**: Artifacts page is complete with all 7 types, envelope format, and lifecycle mapping. Cross-links to constitution and common-workflows pages are correct. `npm run build` passes.

---

## Phase 5: User Story 3 — Complete Workflow Command Documentation (Priority: P2)

**Goal**: Add `/workflow status`, `/workflow list`, and `/workflow advance` documentation to the common-workflows page.

**Independent Test**: Search the common-workflows page for `/workflow status`, `/workflow list`, and `/workflow advance` -- all must appear with usage syntax and behavior descriptions.

### Implementation for User Story 3

- [x] T013 [US3] Add a "Workflow Management" section to content/docs/getting-started/common-workflows.md after the existing "Workflow Configuration" section (currently around line 173). This section documents the commands for monitoring and managing active workflows.
- [x] T014 [US3] Add `/workflow status` subsection to the Workflow Management section in content/docs/getting-started/common-workflows.md. Include: usage syntax (`/workflow status [workflow-id]`), auto-detection from current git branch, displayed information (workflow ID, branch, backlog item, status, start time, iteration count), stage status indicators table (`✓` completed, `◉` active, `○` pending, `⏸` awaiting human, `⊘` skipped, `✗` failed), per-stage details (execution mode, hero name, elapsed time), and the "Run /workflow advance to resume" prompt when awaiting human. Source: research.md R3.
- [x] T015 [US3] Add `/workflow list` subsection to the Workflow Management section in content/docs/getting-started/common-workflows.md. Include: usage syntax (`/workflow list [--status active|completed|all]`), output format (table with ID, Branch, Status, Started columns), sorting (most recent first), and default behavior (shows all workflows). Source: research.md R3.
- [x] T016 [US3] Add `/workflow advance` subsection to the Workflow Management section in content/docs/getting-started/common-workflows.md. Include: usage syntax (`/workflow advance [workflow-id]`), normal advance behavior (completes current stage, activates next), checkpoint handling (pauses at swarm-to-human boundary with `awaiting_human` status), resume behavior (activates next pending human-mode stage), escalation (after 3 review iterations, escalates to human review), and completion behavior (marks workflow "completed", generates `workflow-record` artifact). Source: research.md R3.

**Checkpoint**: All 3 workflow management commands documented. `npm run build` passes.

---

## Phase 6: User Story 4 — Convention Pack Documentation (Priority: P2)

**Goal**: Expand the convention packs section in the developer guide from ~4 lines to a comprehensive subsection covering all 6 pack files, ownership model, customization, and language detection.

**Independent Test**: Read the convention packs section and verify it describes all 6 pack files, ownership model, rule severity tags, language auto-detection, and customization process.

### Implementation for User Story 4

- [x] T017 [P] [US4] Rewrite the "Convention Packs" section in content/docs/getting-started/developer.md. Replace the current ~4-line description with a comprehensive subsection. Start with a brief explanation of what convention packs are: shared coding standards files in `.opencode/unbound/packs/` that Cobalt-Crush follows during implementation and The Divisor enforces during review. Source: research.md R4.
- [x] T018 [US4] Add a Pack Files table to the Convention Packs section in content/docs/getting-started/developer.md. List all 6 files: `default.md` (tool-owned, language-agnostic rules: coding style, architecture, security, testing, documentation), `default-custom.md` (user-owned, project-specific conventions extending defaults), `go.md` (tool-owned, Go-specific: gofmt, error handling, GoDoc, Cobra patterns), `go-custom.md` (user-owned, project-specific Go rules), `typescript.md` (tool-owned, TypeScript-specific: ESLint, Prettier, strict typing, architectural patterns), `typescript-custom.md` (user-owned, project-specific TypeScript rules). Source: research.md R4.
- [x] T019 [US4] Add Ownership Model, Severity Tags, Language Detection, and Customization subsections to the Convention Packs section in content/docs/getting-started/developer.md. Cover: (a) Ownership: tool-owned files updated by `uf init` when embedded version changes; user-owned files (`*-custom.md`) never overwritten. (b) Rule Severity Tags: `[MUST]` (mandatory, violations block review), `[SHOULD]` (strong recommendations), `[MAY]` (optional improvements). (c) Language auto-detection: `go.mod` -> Go, `tsconfig.json`/`package.json` -> TypeScript; only matching language pack deployed; override with `--lang` flag on `uf init`. (d) Adding custom rules: edit `*-custom.md` files, use `CR-NNN` prefix for rule IDs. Source: research.md R4.

**Checkpoint**: Convention packs section is comprehensive. `npm run build` passes.

---

## Phase 7: User Story 5 — `uf init` Standalone Documentation (Priority: P3)

**Goal**: Add a dedicated `uf init` subsection to the developer guide documenting standalone capabilities, flags, file categories, and the ownership model.

**Independent Test**: Read the `uf init` section and verify it describes scaffolded file categories, `--divisor` and `--lang` flags, and the file ownership model.

### Implementation for User Story 5

- [x] T020 [P] [US5] Add a new "Project Scaffolding with `uf init`" section to content/docs/getting-started/developer.md after the Convention Packs section. Include: (a) What `uf init` does: scaffolds project files for working with the Unbound Force swarm; (b) Usage: `uf init [--divisor] [--lang go|typescript] [--force]`; (c) Flags: `--divisor` (deploy only PR review agents and convention packs for projects that only want code review), `--lang` (override language auto-detection), `--force` (overwrite user-owned files, use with caution). Source: research.md R5.
- [x] T021 [US5] Add File Categories and Ownership Model subsections to the `uf init` section in content/docs/getting-started/developer.md. Cover: (a) File categories deployed: agents (~12 files), commands (~15 files), convention packs (6 files), templates (~6 files), scripts (~5 files), OpenSpec schema and templates (~6 files). Frame counts as approximate. (b) File ownership model: tool-owned (commands, skills, canonical packs, schemas -- automatically updated on re-run when content differs; version marker `<!-- scaffolded by uf v{version} -->`) vs. user-owned (agent files, custom packs, specify configs -- never overwritten, skipped if exists). (c) Sub-tool initialization: creates `.unbound-force/config.yaml`; initializes Dewey workspace if available. Source: research.md R5.

**Checkpoint**: `uf init` section is complete. `npm run build` passes.

---

## Phase 8: Polish & Cross-Cutting Concerns

**Purpose**: Build verification, cross-page consistency validation, and cross-link verification.

- [x] T022 Run `npm run build` and verify zero errors after all documentation changes
- [x] T023 Verify cross-page consistency: (a) constitution page links to artifacts page for envelope format details; (b) artifacts page links to constitution page for Principle I and to common-workflows page for lifecycle stages; (c) common-workflows page workflow commands are consistent with existing /workflow seed and /workflow start documentation; (d) developer.md convention packs and uf init sections cross-link to relevant pages. Fix any missing or broken cross-links.
- [x] T024 Verify sidebar navigation: both new pages (constitution, artifacts) appear in the Getting Started sidebar section with correct ordering (artifacts near workflows, constitution near contributing). Verify both are accessible within two clicks from the homepage.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: N/A -- no setup tasks
- **Foundational (Phase 2)**: N/A -- no foundational tasks
- **User Stories (Phases 3-7)**: All can begin immediately, no blocking prerequisites
- **Polish (Phase 8)**: Depends on all user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: No dependencies -- can start immediately. Creates constitution.md (new file).
- **User Story 2 (P1)**: No dependencies -- can start immediately. Creates artifacts.md (new file). Cross-links to constitution page (URL known in advance).
- **User Story 3 (P2)**: No dependencies on US1/US2 -- modifies different sections of common-workflows.md than existing content.
- **User Story 4 (P2)**: No dependencies -- modifies the convention packs section of developer.md.
- **User Story 5 (P3)**: No dependencies on US4 -- adds a new section to developer.md after convention packs. Can be parallel if modifying different sections.

### Within Each User Story

- Tasks without [P] must be executed sequentially (they build on previous content in the same file)
- Tasks with [P] can run in parallel with other [P] tasks in the same phase
- Complete all tasks in a phase before moving to the next

### Parallel Opportunities

- T009 (artifacts.md) can run in parallel with T001-T008 (constitution.md) -- different files
- T017 (developer.md convention packs) can run in parallel with T013-T016 (common-workflows.md)
- T020 (developer.md uf init) can run in parallel with T013-T016 (common-workflows.md)
- All user stories (Phases 3-7) can run in parallel if staffed -- all modify different files or different sections

---

## Parallel Example: Both P1 Stories

```text
# US1 and US2 can run fully in parallel (different files):

# US1 tasks (constitution.md -- new file):
Task: "Create constitution page with frontmatter and introduction"
Task: "Add Principle I: Autonomous Collaboration"
Task: "Add Principle II: Composability First"
...

# US2 tasks (artifacts.md -- new file):
Task: "Create artifacts page with frontmatter and introduction"
Task: "Add Envelope Format section"
Task: "Add Artifact Types table"
Task: "Add Lifecycle Stage Mapping"
```

---

## Implementation Strategy

### MVP First (User Stories 1 + 2 Only)

1. Complete Phase 3: Constitution page (US1)
2. Complete Phase 4: Artifacts page (US2)
3. **STOP and VALIDATE**: `npm run build` + verify sidebar, cross-links, content accuracy
4. Deploy if ready -- fills the two largest documentation gaps

### Incremental Delivery

1. Phases 3-4 (P1): Constitution + Artifacts pages -> Build + Verify -> Deploy
2. Phase 5 (P2): Workflow commands -> Build + Verify -> Deploy
3. Phase 6 (P2): Convention packs expansion -> Build + Verify -> Deploy
4. Phase 7 (P3): `uf init` docs -> Build + Verify -> Deploy
5. Phase 8: Final consistency check -> Deploy

### Parallel Team Strategy

With two workers:

1. Worker A: US1 (constitution.md) + US3 (common-workflows.md) + US5 (developer.md uf init)
2. Worker B: US2 (artifacts.md) + US4 (developer.md convention packs)
3. Both complete -> Phase 8 consistency check

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- All source data for content changes is in research.md (R1-R6)
- Constitution content should be adapted for a website audience -- include rationale paragraphs from the upstream document to explain "why" to visitors
- Artifacts page is technical reference -- use tables for type/producer/consumer mappings
- Workflow command docs should be concise reference style, not tutorial
- Convention pack section replaces the existing ~4 lines, not appends to them
- `uf init` file counts should be framed as approximate (they change between versions)
- Run `npm run build` after each phase as a checkpoint
