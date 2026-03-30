# Tasks: Project Documentation Update

**Input**: Design documents from `/specs/012-project-docs-update/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, quickstart.md

**Tests**: Not applicable. This is a static documentation site. Validation is `npm run build` + visual verification via `npm run dev`.

**Organization**: Tasks are grouped by user story. Stories that modify different files can run in parallel. Within a story, tasks are sequential unless marked `[P]`.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3). Cross-cutting verification tasks (Phase 9) omit this tag since they span multiple stories.
- Include exact file paths in descriptions

---

## Phase 1: User Story 1 — Fix Critical Gaze Errors (Priority: P1)

**Goal**: Eliminate 3 critical factual errors and 1 missing package from the Gaze project page. These are the highest-severity gaps — factually wrong content on a live website.

**Independent Test**: Read `content/docs/projects/gaze.md` and verify: (a) `gaze init` file count is 8, (b) no `/classify-docs` references exist, (c) assertion accuracy reads "~84.7%", (d) `aireport` appears in the architecture table.

- [x] T001 [US1] Update `gaze init` file count from 4 to 8 and list all files across 3 directories (agents, commands, references) in `content/docs/projects/gaze.md` (FR-001)
- [x] T002 [US1] Remove all references to the `/classify-docs` command from `content/docs/projects/gaze.md` — the command was removed in Gaze spec 012 and document-enhanced classification was inlined into the `gaze-reporter` agent (FR-002)
- [x] T003 [US1] Update assertion mapping accuracy from "~78.8%" to "~84.7%" with ratchet floor of 84.0% noted in the Current Limitations section of `content/docs/projects/gaze.md` (FR-003)
- [x] T004 [US1] Add `aireport` package (AI CLI adapter) to the architecture table in `content/docs/projects/gaze.md` (FR-004)

**Checkpoint**: `npm run build` must succeed. Verify gaze.md renders correctly with `npm run dev`.

---

## Phase 2: User Story 2 — Add Missing Gaze Features (Priority: P1)

**Goal**: Document 4 major features absent from the website: `--ai=opencode` adapter, `--coverprofile` flag, `/gaze fix` command, and fix strategy labels.

**Independent Test**: Read `content/docs/projects/gaze.md` and `content/docs/getting-started/tester.md` and verify: (a) `--ai=opencode` appears, (b) `--coverprofile` is documented with the two-step CI pattern, (c) `/gaze fix` command is documented, (d) fix strategy labels are explained.

- [x] T005 [P] [US2] Add `--ai=opencode` as a supported AI adapter alongside `--ai=claude` in the CLI usage section of `content/docs/projects/gaze.md` (FR-005)
- [x] T006 [P] [US2] Add `--ai=opencode` as a supported AI adapter option in `content/docs/getting-started/tester.md` (FR-005)
- [x] T007 [US2] Document the `--coverprofile` flag with the recommended two-step CI pattern (run `go test -coverprofile=coverage.out` separately, then pass to `gaze report`) in `content/docs/getting-started/tester.md` (FR-006)
- [x] T008 [US2] Document the `/gaze fix` command for AI-assisted test generation (reads quality data, generates Go test functions via `gaze-test-generator` agent) in `content/docs/projects/gaze.md` (FR-007)
- [x] T009 [US2] Document fix strategy labels (`add_tests`, `add_assertions`, `decompose`, `decompose_and_test`) with a table explaining each label's meaning in `content/docs/projects/gaze.md` (FR-008)

**Checkpoint**: `npm run build` must succeed. Verify both gaze.md and tester.md render correctly.

---

## Phase 3: User Story 3 — Update Gaze Minor Items (Priority: P2)

**Goal**: Fix stale numbers, unrealistic thresholds, and imprecise limitation descriptions across Gaze documentation pages.

**Independent Test**: Read Gaze pages and verify: (a) CI threshold uses realistic value, (b) single-package limitation is scoped correctly, (c) P3-P4 text does not imply P2 is undetected.

- [x] T010 [P] [US3] Update the CI integration example `--max-crapload` threshold from 15 to 50 with a note that the value depends on the project in `content/docs/getting-started/tester.md` (FR-009)
- [x] T011 [P] [US3] Scope the single-package loading limitation to `gaze analyze` and `gaze quality` only (not `gaze crap` or `gaze report`) in `content/docs/projects/gaze.md` (FR-010)
- [x] T012 [P] [US3] Reword P3-P4 side effect detection text to clearly state P2 IS detected (file system, database, goroutine, panic, context cancellation) and only P3-P4 are undetected in `content/docs/projects/gaze.md` (FR-011)

**Checkpoint**: `npm run build` must succeed. Verify gaze.md and tester.md render correctly.

---

## Phase 4: User Story 4 — Update Dewey Source Configuration and Ignore Behavior (Priority: P1)

**Goal**: Document `.gitignore` support (Spec 006), new `ignore` and `recursive` YAML fields, and correct the stale "indexes all Markdown files" claim.

**Independent Test**: Read `content/docs/getting-started/knowledge.md` and verify: (a) `.gitignore` respect is documented, (b) YAML example includes `ignore` and `recursive` fields, (c) "indexes all Markdown files" claim is updated.

- [x] T013 [US4] Update the Local Disk source section in `content/docs/getting-started/knowledge.md` to document automatic `.gitignore` respect — `.gitignore` at the source root is automatically honored with no configuration needed (FR-012)
- [x] T014 [US4] Add `ignore` and `recursive` fields to the Local Disk YAML configuration example in `content/docs/getting-started/knowledge.md` with syntax documentation and examples (FR-013)
- [x] T015 [US4] Update or remove the claim about indexing "all Markdown files including hidden directories" to reflect `.gitignore` filtering behavior in `content/docs/getting-started/knowledge.md` (FR-014)

**Checkpoint**: `npm run build` must succeed. Verify knowledge.md renders correctly.

---

## Phase 5: User Story 5 — Add Missing Dewey CLI Commands and Troubleshooting (Priority: P2)

**Goal**: Document `dewey doctor`, `dewey reindex`, global CLI flags, and add a troubleshooting section to the knowledge getting-started page.

**Independent Test**: Read `content/docs/getting-started/knowledge.md` and verify: (a) `dewey doctor` is documented, (b) `dewey reindex` is documented, (c) global CLI flags are documented, (d) troubleshooting section covers common issues.

- [x] T016 [US5] Document `dewey doctor` command with its 7 diagnostic sections (environment, workspace, database, sources, embeddings, MCP server, summary) in `content/docs/getting-started/knowledge.md` (FR-015)
- [x] T017 [US5] Document `dewey reindex` command (deletes graph.db and re-indexes from scratch) in `content/docs/getting-started/knowledge.md` (FR-016)
- [x] T018 [US5] Document global CLI flags (`--verbose`/`-v`, `--log-file PATH`, `--no-embeddings`, `--vault PATH`) in `content/docs/getting-started/knowledge.md` (FR-017)
- [x] T019 [US5] Add a Troubleshooting section to `content/docs/getting-started/knowledge.md` covering: MCP server timeout, Ollama not running, lock file conflicts, low embedding coverage, and slow startup — with symptoms and resolution steps for each (FR-018)

**Checkpoint**: `npm run build` must succeed. Verify knowledge.md renders correctly, especially tables in the troubleshooting section.

---

## Phase 6: User Story 6 — Document /unleash and /finale Commands (Priority: P2)

**Goal**: Add documentation for the two flagship workflow commands — `/unleash` (autonomous pipeline) and `/finale` (end-of-branch workflow) — to the common workflows page.

**Independent Test**: Read `content/docs/getting-started/common-workflows.md` and verify: (a) `/unleash` is documented with 8-step pipeline, exit points, and resumability, (b) `/finale` is documented with 9-step workflow and branch safety.

- [x] T020 [US6] Add `/unleash` section to `content/docs/getting-started/common-workflows.md` documenting the 8-step pipeline (clarify, plan, tasks, spec review, implement, code review, retrospective, demo), exit points, Dewey-powered clarification, parallel Swarm workers, resumability, and branch safety (FR-019)
- [x] T021 [US6] Add `/finale` section to `content/docs/getting-started/common-workflows.md` documenting the 9-step workflow (branch safety, check changes, commit message, push, create PR, watch CI, merge, return to main, summary), guardrails, rebase-merge strategy, and relationship to `/unleash` (FR-020)

**Checkpoint**: `npm run build` must succeed. Verify common-workflows.md renders correctly — especially the step tables.

---

## Phase 7: User Story 7 — Document Review Council CI Gate and Divisor Refinements (Priority: P3)

**Goal**: Document Phase 1a/1b CI gate in the code review workflow, update convention pack count to 7, and add Divisor refinements (exclusive ownership, severity standard, prior learnings).

**Independent Test**: Read `content/docs/getting-started/common-workflows.md`, `content/docs/getting-started/developer.md`, and `content/docs/team/the-divisor.md` and verify: (a) Phase 1a/1b documented, (b) 7 convention pack files listed, (c) Divisor refinements documented.

- [x] T022 [P] [US7] Update the Code Review section in `content/docs/getting-started/common-workflows.md` to document Phase 1a (CI hard gate — commands derived from workflow files, including lint and vulnerability check) and Phase 1b (conditional Gaze quality analysis passed as context to reviewers) (FR-021)
- [x] T023 [P] [US7] Update the convention packs table in `content/docs/getting-started/developer.md` to list 7 files (add `severity.md` — tool-owned, shared severity definitions for all Divisor personas as the calibration standard for CRITICAL/HIGH/MEDIUM/LOW findings) (FR-022)
- [x] T024 [P] [US7] Update `content/docs/team/the-divisor.md` to document: (a) exclusive ownership boundaries (each review dimension owned by exactly one persona, with ownership table), (b) shared severity standard reference (`severity.md` pack), (c) prior learnings integration (Hivemind search before review, graceful degradation) (FR-023)

**Checkpoint**: `npm run build` must succeed. Verify all three pages render correctly.

---

## Phase 8: User Story 8 — Update uf init and uf setup Documentation (Priority: P3)

**Goal**: Reflect that `opencode.json` management moved from `uf setup` to `uf init`, and update step counts accordingly.

**Independent Test**: Read `content/docs/getting-started/common-workflows.md` and `content/docs/getting-started/developer.md` and verify: (a) `uf setup` step count is updated, (b) `uf init` mentions `opencode.json` management, (c) `--force` flag description includes Dewey re-indexing.

- [x] T025 [P] [US8] Update the `uf init` documentation in `content/docs/getting-started/developer.md` to include `opencode.json` management: Dewey MCP server entry (when dewey is in PATH), Swarm plugin entry (when `.hive/` exists), idempotent behavior, and `--force` flag effect on Dewey re-indexing (FR-024)
- [x] T026 [P] [US8] Update the `uf setup` documentation in `content/docs/getting-started/common-workflows.md` to reflect the reduced step count after the `opencode.json` move to `uf init` (FR-025)

**Checkpoint**: `npm run build` must succeed. Verify both pages render correctly.

---

## Phase 9: Cross-Cutting Verification

**Purpose**: Validate cross-page consistency and overall build integrity after all content changes.

- [x] T027 Search all content pages for any remaining references to `/classify-docs` and remove them (FR-002, FR-027)
- [x] T028 Verify cross-page consistency: convention pack count (7), assertion accuracy (~84.7%), `gaze init` file count (8), AI adapters (opencode and claude), `--max-crapload` (50) — no contradictory claims between pages (FR-027)
- [x] T029 Run `npm run build` and verify zero errors (FR-026)
- [x] T030 Run quickstart.md verification steps (Steps 1-9) to validate all acceptance scenarios

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (US1)**: No dependencies — can start immediately. Highest priority (P1).
- **Phase 2 (US2)**: No dependencies on Phase 1 — can start in parallel (different content sections within gaze.md, plus tester.md). Highest priority (P1).
- **Phase 3 (US3)**: No strict dependency on Phase 1/2 but touches same files (gaze.md, tester.md). Run after Phase 1 and 2 to avoid merge conflicts.
- **Phase 4 (US4)**: No dependencies — modifies only knowledge.md. Can run in parallel with Phases 1-3. Highest priority (P1).
- **Phase 5 (US5)**: Depends on Phase 4 (same file: knowledge.md). Run after Phase 4.
- **Phase 6 (US6)**: No dependencies — modifies only common-workflows.md. Can run in parallel with Phases 1-5.
- **Phase 7 (US7)**: No strict dependency but touches common-workflows.md (also Phase 6), developer.md, and the-divisor.md. Run after Phase 6 for common-workflows.md tasks.
- **Phase 8 (US8)**: Touches developer.md (also Phase 7) and common-workflows.md (also Phases 6-7). Run after Phase 7.
- **Phase 9 (Verification)**: Depends on ALL previous phases being complete.

### Parallel Opportunities by File

Tasks modifying **different files** can run in parallel:

| File                  | Tasks                                 |
| --------------------- | ------------------------------------- |
| `gaze.md`             | T001-T004, T005, T008-T009, T011-T012 |
| `tester.md`           | T006, T007, T010                      |
| `knowledge.md`        | T013-T019                             |
| `common-workflows.md` | T020-T022, T026                       |
| `developer.md`        | T023, T025                            |
| `the-divisor.md`      | T024                                  |

**Maximum parallelism**: 4 file groups can proceed simultaneously:

1. `gaze.md` (US1 + US2 + US3 tasks)
2. `tester.md` (US2 + US3 tasks)
3. `knowledge.md` (US4 + US5 tasks)
4. `the-divisor.md` (US7 task)

After those complete: 5. `common-workflows.md` (US6 + US7 + US8 tasks) 6. `developer.md` (US7 + US8 tasks)

### Within Each File

Tasks within the same file are sequential (to avoid edit conflicts) unless marked `[P]` — which indicates they modify different sections of the same file.

---

## Implementation Strategy

### Recommended Execution Order (Single Developer)

1. **P1 stories first** (Phases 1, 2, 4): Fix critical errors and add missing features
2. **P2 stories next** (Phases 3, 5, 6): Minor fixes, Dewey CLI, workflow commands
3. **P3 stories last** (Phases 7, 8): Review council, Divisor, uf init/setup updates
4. **Verification** (Phase 9): Cross-page consistency and build validation

### Parallel Team Strategy

With multiple developers:

- **Developer A**: gaze.md + tester.md (Phases 1, 2, 3)
- **Developer B**: knowledge.md (Phases 4, 5)
- **Developer C**: common-workflows.md + developer.md + the-divisor.md (Phases 6, 7, 8)
- **All**: Phase 9 verification after merge

---

## Notes

- [P] tasks = different files or different sections, no dependencies
- [Story] label maps task to specific user story for traceability
- No tests required — validation is `npm run build` + visual verification
- Commit after each phase or logical group
- Stop at any checkpoint to validate independently
- All content must be sourced from research.md — never fabricate features or overstate maturity
<!-- spec-review: passed -->
