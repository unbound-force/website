# Tasks: Binary Rename -- Website Updates

**Input**: Design documents from `/specs/006-binary-rename/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md

**Tests**: No test tasks included -- the spec does not request automated tests. Validation is manual: `npm run build`, visual verification, and content grep.

**Organization**: Tasks are grouped by user story. All three stories operate on separate files and can run in parallel.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

---

## Phase 1: User Story 1 - Developer Reads Accurate CLI Commands (Priority: P1) MVP

**Goal**: Update all 8 CLI binary references in the Developer Environment Setup page so every install command and CLI subcommand uses the correct renamed binary (`uf` / `unbound-force`).

**Independent Test**: Navigate to the Developer Environment Setup page, read every CLI command, and confirm each one matches the renamed binary. The first CLI mention explains that `uf` is short for `unbound-force`.

### Implementation for User Story 1

- [x] T001 [P] [US1] Update the introductory CLI reference from `unbound` to `uf` with alias explanation (FR-002, FR-003) in content/docs/getting-started/developer.md line 13: change ``Install the `unbound` CLI, then run setup`` to ``Install the `uf` CLI (short for `unbound-force`), then run setup``
- [x] T002 [P] [US1] Update the Homebrew install command (FR-001) in content/docs/getting-started/developer.md line 16: change `brew install unbound-force/tap/unbound` to `brew install unbound-force/tap/unbound-force`
- [x] T003 [P] [US1] Update the setup command in the code block (FR-002) in content/docs/getting-started/developer.md line 17: change `unbound setup` to `uf setup`
- [x] T004 [P] [US1] Update the inline setup reference (FR-002) in content/docs/getting-started/developer.md line 20: change `` `unbound setup` `` to `` `uf setup` ``
- [x] T005 [P] [US1] Update the doctor command in the code block (FR-002) in content/docs/getting-started/developer.md line 30: change `unbound doctor` to `uf doctor`
- [x] T006 [P] [US1] Update the inline doctor reference in the daily workflow section (FR-002) in content/docs/getting-started/developer.md line 39: change `` `unbound doctor` `` to `` `uf doctor` ``
- [x] T007 [P] [US1] Update the init reference in the convention packs section (FR-002) in content/docs/getting-started/developer.md line 138: change `` `unbound init` `` to `` `uf init` ``
- [x] T008 [P] [US1] Update the doctor reference in the next steps section (FR-002) in content/docs/getting-started/developer.md line 163: change `` `unbound doctor` `` to `` `uf doctor` ``

**Checkpoint**: Developer Environment Setup page shows correct CLI commands. All 8 references updated. Page builds without errors.

---

## Phase 2: User Story 2 - Developer Follows Accurate Workflow Documentation (Priority: P2)

**Goal**: Update all 4 CLI binary references in the Common Workflows page (Environment Setup section) and add an alias explanation at the first CLI mention.

**Independent Test**: Read the Environment Setup section of the Common Workflows page and confirm every CLI command matches the renamed binary. The section heading area explains that `uf` is short for `unbound-force`.

### Implementation for User Story 2

- [x] T009 [P] [US2] Add alias explanation and update the Homebrew install command (FR-001, FR-003) in content/docs/getting-started/common-workflows.md lines 191-194: add a brief note that `uf` (short for `unbound-force`) is the CLI tool, and change `brew install unbound-force/tap/unbound` to `brew install unbound-force/tap/unbound-force`
- [x] T010 [P] [US2] Update the setup command in the code block (FR-002) in content/docs/getting-started/common-workflows.md line 200: change `unbound setup` to `uf setup`
- [x] T011 [P] [US2] Update the inline init reference (FR-002) in content/docs/getting-started/common-workflows.md line 211: change `` `unbound init` `` to `` `uf init` ``
- [x] T012 [P] [US2] Update the doctor command in the code block (FR-002) in content/docs/getting-started/common-workflows.md line 218: change `unbound doctor` to `uf doctor`

**Checkpoint**: Common Workflows Environment Setup section shows correct CLI commands. All 4 references updated plus alias note added. Page builds without errors.

---

## Phase 3: User Story 3 - Product Manager Reads Correct Install Instructions (Priority: P3)

**Goal**: Update both CLI references in the Product Manager guide so the Homebrew install command and package name use the renamed binary.

**Independent Test**: Read the Product Manager page installation section and confirm the Homebrew command and package name reference are correct.

### Implementation for User Story 3

- [x] T013 [P] [US3] Update the package name reference with alias explanation (FR-002, FR-003) in content/docs/getting-started/product-manager.md line 31: change ``included with the `unbound` package`` to ``included with the `unbound-force` package (the `uf` CLI)``
- [x] T014 [P] [US3] Update the Homebrew install command (FR-001) in content/docs/getting-started/product-manager.md line 34: change `brew install unbound-force/tap/unbound` to `brew install unbound-force/tap/unbound-force`

**Checkpoint**: Product Manager page shows correct install command and package name. Both references updated. Page builds without errors.

---

## Phase 4: Verification & Cross-Cutting Concerns

**Purpose**: Validate all changes across all three files, confirm no residual `unbound` CLI references remain, and verify the site builds cleanly.

- [x] T015 Run `npm run build` and confirm zero build errors (FR-006, SC-003)
- [x] T016 Search the built `public/` output for any remaining bare `unbound` CLI command references, excluding organization name references (FR-007, SC-001)
- [x] T017 Visually verify all 3 updated pages render correctly via `npm run dev` (SC-002)
- [x] T018 Verify all 3 updated pages render correctly in dark mode

**Checkpoint**: All user stories verified. Site builds cleanly. Zero residual `unbound` CLI references. Ready for PR.

---

## Dependencies & Execution Order

### Phase Dependencies

- **User Stories (Phases 1-3)**: No dependencies on each other -- all operate on different files and can start immediately
- **Verification (Phase 4)**: Depends on all three user story phases being complete

### User Story Dependencies

- **User Story 1 (P1)**: No dependencies -- edits only `developer.md`
- **User Story 2 (P2)**: No dependencies -- edits only `common-workflows.md`
- **User Story 3 (P3)**: No dependencies -- edits only `product-manager.md`

### Within Each User Story

- All tasks within a story are marked [P] because they edit different lines in the same file with no interdependencies
- In practice, tasks within a single file are best applied sequentially to avoid merge conflicts, but they have no logical dependency on each other

### Parallel Opportunities

- All three user stories can run in parallel (different files)
- A single implementer should work through stories sequentially (P1 then P2 then P3) for clean commits
- With multiple implementers: each takes one story file

---

## Parallel Example: All User Stories

```bash
# All three stories can launch in parallel since they edit different files:
Task: "Update CLI references in content/docs/getting-started/developer.md (US1)"
Task: "Update CLI references in content/docs/getting-started/common-workflows.md (US2)"
Task: "Update CLI references in content/docs/getting-started/product-manager.md (US3)"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: User Story 1 (developer.md -- the most critical page)
2. **STOP and VALIDATE**: Run `npm run build`, visually verify developer.md
3. The most important page is now correct -- deploy if needed

### Incremental Delivery

1. Complete US1 (developer.md) -- MVP, highest impact
2. Complete US2 (common-workflows.md) -- second most visited page
3. Complete US3 (product-manager.md) -- smallest change, lowest risk
4. Run full verification (Phase 4) -- build, grep, visual, dark mode
5. Each story adds consistency without breaking previous stories

---

## Notes

- All [P] tasks operate on different files or different lines -- no conflicts
- No automated tests are needed; validation is manual build + visual + grep
- Organization name references ("Unbound Force" in prose, GitHub URLs) MUST NOT be changed (FR-005)
- Line numbers in task descriptions are based on current file content and may shift if tasks are applied out of order within the same file
- Commit after completing each user story phase for clean git history
