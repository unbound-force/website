# Tasks: Replicator Migration

**Branch**: `016-replicator-migration` | **Date**: 2026-04-06

## Dependencies

- Phase 1 (US3 - New Page) has no dependencies
- Phase 2 (US1+US2 - Content Updates) tasks are independent of each other (`[P]`)
- Phase 3 (US4 - Internal Files) tasks are independent of each other (`[P]`)
- Phase 4 (US5 - Config) has no dependencies
- Phase 5 (Verification) depends on all prior phases

## Phase 1: New Content (US3)

- [x] T001 [US3] Create Replicator project page at `content/docs/projects/replicator.md` with installation, features (53 MCP tools across 4 categories), CLI commands, architecture, and heritage attribution (FR-010, FR-011)
- [x] T002 [US3] Add Replicator entry to projects index page `content/docs/projects/_index.md` (FR-013)
- [x] T003 [US3] Add Replicator card to homepage `layouts/home.html` — change grid from `col-lg-6` to `col-lg-4`, add 3rd project card with Go badge, MCP Server type, benefit-framed description (FR-012)

## Phase 2: Content Updates (US1 + US2)

- [x] T004 [P] [US1][US2] Update `content/docs/getting-started/quick-start.md` — replace "Swarm plugin" with "Replicator" in setup list, replace Stack table Coordination row with Replicator, remove `/swarm` command block, update "coordinate with Swarm" text (FR-001, FR-002, FR-005, FR-008, FR-009)
- [x] T005 [P] [US1][US2] Update `content/docs/getting-started/developer.md` — replace "Swarm" in lead, replace "Swarm plugin" in setup list, rename "Working with Swarm" section to "Working with Replicator", replace `/swarm` command references, update opencode.json documentation from plugin array to mcp.replicator entry, update "Swarm plugin health" reference (FR-001, FR-003, FR-004, FR-006, FR-007, FR-008, FR-009)
- [x] T006 [P] [US1][US2] Update `content/docs/getting-started/common-workflows.md` — replace "Swarm plugin" in setup list, replace "Swarm workers" product references (keep generic "swarm" nouns), remove `/swarm` command references, replace "Install the Swarm plugin" fallback message, update "Swarm plugin entries" reference (FR-001, FR-003, FR-008, FR-009)
- [x] T007 [P] [US1] Update `content/docs/getting-started/_index.md` — replace Stack table Coordination row with Replicator, update "coordinate with Swarm" text (FR-001, FR-002, FR-005)
- [x] T008 [P] [US1] Update `content/docs/getting-started/knowledge.md` — replace "swarm init" with "replicator init" (FR-004)
- [x] T009 [P] [US1] Verify `content/docs/getting-started/artifacts.md` — confirm "Swarm Orchestration" is a role name (not product reference) and keep unchanged (FR-019)
- [x] T010 [P] [US1] Update `content/blog/unleash-in-practice.md` — replace "Swarm" product references with "Replicator" where referring to the tool, keep generic "swarm" nouns, update Swarm link in implement section (FR-001, FR-002)

## Phase 3: Internal Files (US4)

- [x] T011 [P] [US4] Update `.opencode/command/unleash.md` — replace "Swarm plugin" with "Replicator" in all fallback messages and install hints (FR-014)
- [x] T012 [P] [US4] Update `.opencode/agents/cobalt-crush-dev.md` — replace "Swarm" product references with "Replicator" in coordination section header and description (FR-015)
- [x] T013 [P] [US4] Update `.opencode/skill/speckit-workflow/SKILL.md` — replace "Swarm coordinator" with "Replicator" in description and body (FR-016)

## Phase 4: Configuration (US5)

- [x] T014 [US5] Remove `web-swarm` entry from `.dewey/sources.yaml` (FR-017)

## Phase 5: Verification

- [x] T015 Run `npm run build` and verify zero errors (FR-018)
- [x] T016 Verify `grep -r "swarmtools.ai" content/ layouts/` returns zero results — heritage uses GitHub links not swarmtools.ai (SC-001)
- [x] T017 Verify `grep -r "opencode-swarm-plugin" content/ layouts/ .opencode/` returns zero results (SC-002)
- [x] T018 Verify `grep -r '"/swarm ' content/` returns zero results (SC-003)
- [x] T019 Verify generic "swarm" noun usage is preserved — 47+ instances intact (FR-019, SC-006)
