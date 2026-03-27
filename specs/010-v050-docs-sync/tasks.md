# Tasks: v0.5.0 Documentation Sync

**Input**: Design documents from `/specs/010-v050-docs-sync/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, quickstart.md

**Tests**: No automated tests -- this is a static documentation site. Validation is `npm run build` + visual verification.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: No setup needed -- all target files already exist. This is a content-only update to existing Markdown files.

_(No tasks in this phase)_

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: No foundational blocking work. All user stories modify independent files and can proceed immediately. The only cross-cutting concern is that `common-workflows.md` is modified by multiple stories, but the changes target different sections of the file.

_(No tasks in this phase)_

**Checkpoint**: All user stories can begin immediately.

---

## Phase 3: User Story 1 - Setup Documentation Reflects Full Toolchain (Priority: P1)

**Goal**: Update `uf setup` documentation to accurately reflect the v0.5.0 16-step setup process, update `uf init` documentation, and consolidate the Dewey/Ollama installation into the setup flow.

**Independent Test**: Read the environment setup section of `common-workflows.md` and `developer.md` and verify every tool from research.md R1 is mentioned. Verify the separate "Install Dewey" step is replaced with a note that `uf setup` now handles it.

### Implementation for User Story 1

- [x] T001 [US1] Rewrite the "2. Run Setup" section in content/docs/getting-started/common-workflows.md to list all tools installed by v0.5.0's `uf setup`, grouped into categories: core tools (OpenCode, Gaze, Mx F, GitHub CLI), development tools (Node.js, Bun, OpenSpec CLI, Swarm plugin + config), knowledge layer (Ollama, Dewey, embedding model, Dewey initialization), and project scaffolding (`uf init`). Source: research.md R1.
- [x] T002 [US1] Replace the separate "3. Install Dewey" section in content/docs/getting-started/common-workflows.md with a note explaining that `uf setup` now handles Dewey and Ollama installation automatically. Keep a brief mention that for pre-v0.5.0 installations, users can re-run `uf setup` to pick up new tools. Add the embedding alignment env vars (`OLLAMA_MODEL`, `OLLAMA_EMBED_DIM`) as a post-setup shell profile step. Renumber subsequent sections (current "4. Verify" becomes "3. Verify", current "5. Start Working" becomes "4. Start Working").
- [x] T003 [US1] Add a brief note after the setup section in content/docs/getting-started/common-workflows.md explaining that `uf init` (run as the final step of setup) now creates `.unbound-force/config.yaml` for workflow configuration and initializes Dewey when available. Source: research.md R2.
- [x] T004 [P] [US1] Update the `uf setup` description in content/docs/getting-started/developer.md (currently lists 4 things: OpenCode, Gaze, Swarm plugin, project scaffolding) to match v0.5.0 behavior. Add Mx F, GitHub CLI, OpenSpec CLI, Ollama, and Dewey to the list. Keep it concise -- this is a summary, not the full breakdown. Source: research.md R1.

**Checkpoint**: Setup documentation accurately reflects v0.5.0. `npm run build` passes.

---

## Phase 4: User Story 2 - Workflow Commands and Configuration Documented (Priority: P1)

**Goal**: Document the `/workflow seed` command, `/workflow start` flags, and `.unbound-force/config.yaml` configuration file so users can invoke the autonomous define capabilities.

**Independent Test**: Search `common-workflows.md` and `product-owner.md` for `/workflow seed`, `--define-mode`, `--spec-review`, and `.unbound-force/config.yaml` -- all must appear with accurate descriptions.

### Implementation for User Story 2

- [x] T005 [US2] Add a "Workflow Commands" subsection to the "Swarm Delegation" section in content/docs/getting-started/common-workflows.md documenting: (a) `/workflow seed "description" [--spec-review]` -- creates a backlog item and starts a workflow with define=swarm in one operation; (b) `/workflow start --define-mode=human|swarm --spec-review` -- starts a workflow with explicit mode selection. Source: research.md R7.
- [x] T006 [US2] Add a "Workflow Configuration" subsection after the Workflow Commands in content/docs/getting-started/common-workflows.md documenting `.unbound-force/config.yaml`: show the YAML structure (`workflow.execution_modes.define`, `workflow.spec_review`), explain that config values are defaults and CLI flags override, and note that the file is created by `uf init` with all values commented out. Source: research.md R7.
- [x] T007 [P] [US2] Add a `/workflow seed` command reference to content/docs/getting-started/product-owner.md in the "Stage 1: Seed" section. After the seed examples, add a brief command reference showing the syntax: `/workflow seed "description" [--spec-review]`. Note that this is a convenience command equivalent to creating a backlog item + `/workflow start --define-mode=swarm`. Source: research.md R7.

**Checkpoint**: All workflow commands and configuration are documented. `npm run build` passes.

---

## Phase 5: User Story 3 - The Divisor Accurately Described as Five Personas (Priority: P2)

**Goal**: Update The Divisor team page and team index to accurately describe five sub-personas instead of three.

**Independent Test**: Read `team/the-divisor.md` and `team/_index.md` and verify five personas are listed. Cross-reference against `common-workflows.md` (already has five) to ensure consistency.

### Implementation for User Story 3

- [x] T008 [US3] Rewrite the council section in content/docs/team/the-divisor.md to describe five sub-personas instead of three. Update: (a) the intro paragraph ("council of three" → "council of five dynamically discovered personas -- five canonical roles ship by default"); (b) keep the existing Guard, Architect, Adversary subsections with their current content; (c) add new subsections for "The Operator (SRE) -- Deployment and Operational Readiness" and "The Tester -- Test Quality and Testability" with descriptions from research.md R4; (d) update the "Collective Approval" section to reference "all five lenses" instead of "all three lenses"; (e) update the "How The Divisor Serves the Team" section's Cobalt-Crush paragraph to reference "five personas" instead of "three personas"; (f) update the frontmatter description to reference five sub-personas. Source: research.md R4.
- [x] T009 [P] [US3] Update content/docs/team/\_index.md: change The Divisor's description from "council of three sub-personas -- The Guard, The Architect, and The Adversary" to "council of five sub-personas -- The Guard, The Architect, The Adversary, The Operator (SRE), and The Tester". Source: research.md R4.

**Checkpoint**: The Divisor is consistently described as five personas across all pages. `npm run build` passes.

---

## Phase 6: User Story 4 - Embedding Model Alignment Documented (Priority: P2)

**Goal**: Document the `OLLAMA_MODEL` and `OLLAMA_EMBED_DIM` environment variables for embedding model alignment between the swarm and Dewey.

**Independent Test**: Search `knowledge.md` for `OLLAMA_MODEL` and `OLLAMA_EMBED_DIM` -- both must appear with shell profile instructions and an explanation of why alignment matters.

### Implementation for User Story 4

- [x] T010 [US4] Add an "Embedding Model Alignment" section to content/docs/getting-started/knowledge.md after the "Embedding Model" section. Explain that v0.5.0 aligns the swarm and Dewey on the same embedding model (IBM Granite `granite-embedding:30m`). Include the shell profile instructions: `export OLLAMA_MODEL=granite-embedding:30m` and `export OLLAMA_EMBED_DIM=256`. Explain that `uf setup` sets these during installation, but they must be in the shell profile for processes spawned outside of setup (e.g., `dewey serve`, manual `swarm init`). Source: research.md R5.

**Checkpoint**: Embedding alignment is documented. `npm run build` passes.

---

## Phase 7: User Story 5 - Branch Conventions in User-Facing Docs (Priority: P3)

**Goal**: Document the `opsx/<change-name>` branch naming convention and branch enforcement behavior for OpenSpec workflows.

**Independent Test**: Read the bug fix / OpenSpec sections of `common-workflows.md` and `developer.md` for `opsx/` branch documentation.

### Implementation for User Story 5

- [x] T011 [US5] Add branch convention documentation to the "Bug Fix (Tactical)" / OpenSpec section of content/docs/getting-started/common-workflows.md. After the existing `/opsx-propose` description, add a note that `/opsx-propose` creates and checks out an `opsx/<name>` branch, `/opsx-apply` validates the current branch before proceeding (hard gate), and `/opsx-archive` returns to `main` after archiving. Mention the `opsx/` prefix ensures visual distinction from Speckit branches (`NNN-<short-name>`). Source: research.md R6.
- [x] T012 [P] [US5] Add a brief branch convention note to content/docs/getting-started/developer.md under the "Speckit vs OpenSpec" section. Note that Speckit uses `NNN-<short-name>` branches (created by `/speckit.specify`) and OpenSpec uses `opsx/<change-name>` branches (created by `/opsx-propose`). Both enforce branch validation at each pipeline step. Source: research.md R6.

**Checkpoint**: Branch conventions documented in user-facing content. `npm run build` passes.

---

## Phase 8: User Story 6 - Dewey MCP Tool Count Consistency (Priority: P3)

**Goal**: Reconcile Dewey MCP tool counts across all pages to use the canonical "40 tools across 10 categories" from the Dewey README.

**Independent Test**: Search the entire site for MCP tool count mentions and verify consistency.

### Implementation for User Story 6

- [x] T013 [P] [US6] Fix MCP tool count in content/docs/team/dewey.md: change "40+ MCP tools across 10 categories" to "40 MCP tools across 10 categories" (remove the "+"). Source: research.md R3.
- [x] T014 [P] [US6] Fix MCP tool count in content/docs/projects/dewey.md: (a) update "preserving all 37 existing MCP tools" to contextualize the graphthulhu heritage while noting the current count; (b) change "40 MCP Tools Across 9 Categories" heading to "40 MCP Tools Across 10 Categories". Source: research.md R3.

**Checkpoint**: All MCP tool count references are consistent. `npm run build` passes.

---

## Phase 9: Polish & Cross-Cutting Concerns

**Purpose**: Build verification, cross-page consistency validation, and visual check.

- [x] T015 Run `npm run build` and verify zero errors after all documentation changes
- [x] T016 Run cross-page consistency checks: search for Divisor persona count references (all must say "five"), MCP tool counts (all must say "40 tools across 10 categories"), and `uf setup` descriptions (all must reflect v0.5.0 behavior). Fix any remaining inconsistencies.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: N/A -- no setup tasks
- **Foundational (Phase 2)**: N/A -- no foundational tasks
- **User Stories (Phases 3-8)**: All can begin immediately, no blocking prerequisites
- **Polish (Phase 9)**: Depends on all user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: No dependencies -- can start immediately
- **User Story 2 (P1)**: No dependencies -- can start immediately (modifies different sections of common-workflows.md than US1)
- **User Story 3 (P2)**: No dependencies -- modifies team pages only
- **User Story 4 (P2)**: No dependencies -- modifies knowledge.md only
- **User Story 5 (P3)**: No dependencies -- modifies different sections than US1/US2
- **User Story 6 (P3)**: No dependencies -- modifies Dewey pages only

### Within Each User Story

- Tasks without [P] must be executed sequentially (they modify the same file in dependent sections)
- Tasks with [P] can run in parallel with other [P] tasks in the same phase
- Complete all tasks in a phase before moving to the next

### Parallel Opportunities

- T004 (developer.md) can run in parallel with T001-T003 (common-workflows.md)
- T007 (product-owner.md) can run in parallel with T005-T006 (common-workflows.md)
- T009 (team/\_index.md) can run in parallel with T008 (team/the-divisor.md)
- T012 (developer.md) can run in parallel with T011 (common-workflows.md)
- T013 (team/dewey.md) and T014 (projects/dewey.md) can run in parallel with each other
- All user stories (Phases 3-8) can run in parallel if staffed

---

## Parallel Example: All P1 Stories

```bash
# US1 and US2 can run fully in parallel since they modify different sections:

# US1 tasks (common-workflows.md setup section + developer.md):
Task: "Rewrite uf setup section in common-workflows.md"
Task: "Update developer.md setup description"  # [P] with above

# US2 tasks (common-workflows.md delegation section + product-owner.md):
Task: "Add workflow commands to common-workflows.md"
Task: "Add /workflow seed to product-owner.md"  # [P] with above
```

---

## Implementation Strategy

### MVP First (User Stories 1 + 2 Only)

1. Complete Phase 3: User Story 1 (setup docs)
2. Complete Phase 4: User Story 2 (workflow commands)
3. **STOP and VALIDATE**: `npm run build` + verify setup and workflow pages
4. Deploy if ready -- users can now follow accurate setup docs and use new commands

### Incremental Delivery

1. Phases 3-4 (P1): Setup + Workflow Docs → Build + Verify → Deploy
2. Phases 5-6 (P2): Divisor + Embedding → Build + Verify → Deploy
3. Phases 7-8 (P3): Branches + Tool Counts → Build + Verify → Deploy
4. Phase 9: Final consistency check → Deploy

### Parallel Team Strategy

With two workers:

1. Worker A: US1 (common-workflows.md setup) + US3 (divisor pages) + US5 (branch conventions)
2. Worker B: US2 (workflow commands) + US4 (knowledge.md) + US6 (dewey tool counts)
3. Both complete → Phase 9 consistency check

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- All source data for content changes is in research.md (R1-R8)
- common-workflows.md is the most-modified file (US1, US2, US5 modify different sections)
- Commit after each phase to maintain clean history
- Run `npm run build` after each phase as a checkpoint
